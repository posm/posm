# Manual Steps

For the purpose of these steps, we're provisioning data for a past project in
Dvizarasekwa, Zimbabwe. The _Deployment AOI_ is `[30.904111862182614,
-17.821343531895728, 30.95578193664551, -17.778602961844793]`.

## Initial Data Gathering

### Generate MBTiles

```bash
nvm use 5
npm install tl tilelive-http mbtiles
node_modules/.bin/tl copy \
  http://a.tile.openstreetmap.org/{z}/{x}/{y}.png \
  mbtiles://./osm_dvizarasekwa_z14-19.mbtiles \
  -b "30.904111862182614 -17.821343531895728 30.95578193664551 -17.778602961844793" \
  -z 14 \
  -Z 19
```

### Fetch OSM XML

```bash
curl -XPOST \
  --compressed \
  -d "data=(node(-17.821343531895728, 30.904111862182614, -17.778602961844793, 30.95578193664551); <; >>; >;); out meta qt;" \
  http://overpass-api.de/api/interpreter > dvizarasekwa.osm
```

### Convert OSM XML → PBF

```bash
osmosis --read-xml dvizarasekwa.osm --wb dvizarasekwa.pbf
```

### Obtain XLS Form

Initial version of ODK Collect survey.

### Create a data manifest describing everything

* References to outputs from above
* (Local) path to XLS (within archive)
* (Local) path(s) to MBTiles
* (Local) path to OSM PBF
* Bounding box
* Name
* Imagery sources w/ zoom levels

(Styles are sketched out here, but we don't intend to use them in the short-term.)

E.g.:

```json
{
  "name": "Dvizarasekwa, Zimbabwe",
  "description": "",
  "bbox": [30.904111862182614, -17.821343531895728, 30.95578193664551, -17.778602961844793],
  "contents": {
    "tiles/osm_dvizarasekwa_z14-19.mbtiles": {
      "type": "MBTiles",
      "minzoom": 14,
      "maxzoom": 19,
      "bbox": "(optional to override the overall bbox)",
      "source": "http://tile.openstreetmap.org/{z}/{x}/{y}.png"
    },
    "tiles/satellite_dvizarasekwa_z14-19.mbtiles": {
      "type": "MBTiles",
      "minzoom": 14,
      "maxzoom": 19,
      "bbox": "(optional to override the overall bbox)",
      "source": "http://tile.digitalglobe.com/something/{z}/{x}/{y}.png"
    },
    "odk/form.xls": {
      "type": "ODKForm"
    },
    "osm/dvizarasekwa.pbf": {
      "type": "OSM/PBF"
    },
    "styles/hdm.xml": {
      "type": "Mapnik/XML"
    },
    "styles/minimal.yaml": {
      "type": "Mapnik/YAML"
    }
  },
  "derivatives": {
    "Buildings and Roads": {
      "type": "OSM/XML",
      "bbox": "(optional, to restrict area covered)",
      "filters": [
        "building=*",
        "highway=*"
      ]
    },
    "HDM Tiles": {
      "type": "MBTiles",
      "bbox": "(optional)",
      "source": "mapnik://styles/hdm.xml"
    }
  }
}
```

## Initial Data Deployment

```bash
sudo -s
mkdir -p /opt/data/tiles
mkdir -p /opt/data/osm
mv /path/to/dvizarasekwa_z14-19.mbtiles /opt/data/tiles
mv /path/to/dvizarasekwa.pbf /opt/data/osm
```

To be replaced by something that reads the data manifest from the archive that's
been downloaded and handles `contents` and `derivatives` accordingly.

### Populate APIDB

Assuming that an APIDB was already populated, it will need to be dropped and
recreated in order to avoid duplicate key errors:

```bash
sudo -u postgres dropdb macrocosm_production
sudo -u postgres createdb --owner macrocosm macrocosm_production
sudo -u postgres psql -d macrocosm_production -c "CREATE EXTENSION btree_gist"
sudo -u macrocosm psql -d macrocosm_production -f /opt/macrocosm/db-server/script/macrocosm-db.sql
sudo -u macrocosm osmosis --read-pbf-fast /opt/data/osm/dvizarasekwa.pbf \
  --log-progress \
  --write-apidb password=macrocosm database=macrocosm_production
```

### Populate Rendering DB

```bash
mem=$(awk 'NR == 1 { print int($2*.9/1024) } ' /proc/meminfo)
sudo -u gis \
  osm2pgsql \
    --create \
    --hstore-all \
    --hstore-add-index \
    --extra-attributes \
    --slim \
    --drop \
    --unlogged \
    --database='gis' \
    -C $mem \
    --number-processes $(nproc) /opt/data/osm/dvizarasekwa.pbf
```

### Restart Tile Server

```bash
sudo -s
cat <<EOF > /etc/tessera.conf.json
{
  "/tiles/mm": "mapnik://./project.xml",
  "/tiles/osm": "mbtiles:///opt/data/tiles/osm_dvizarasekwa_z14-19.mbtiles"
}
EOF
service tessera restart
```

OSM tiles will now be available at
[`posm.local/tiles/osm/`](http://posm.local/tiles/osm/) (hostname dependent on
your configuration). It will auto-center to the center of the deployment AOI.

Locally-rendered tiles will now be available at
[`posm.local/tiles/mm/#16/-17.7990/30.9290`](http://posm.local/tiles/mm/#16/-17.799/30.929)
(again, hostname dependent on your configuration). The location in the URL hash
is required until the map style is correctly configured with a center.

## Field Papers

Adjust hostnames (if necessary) and add AWS keys / bucket info to
`/etc/init/fp-{tasks,tiler,web}.conf`. (Yes, AWS keys imply internet access;
working on that.)

Restart Field Papers services:

```bash
sudo service fp-tasks restart
sudo service fp-tiler restart
sudo service fp-web restart
```

Field Papers will now be available at [`posm.local/fp`](http://posm.local/fp).

### Atlas Generation

When selecting an area, use `http://posm.local/tiles/mm/{z}/{x}/{y}.png` as the
tile source in order to use locally-rendered tiles. If you'd like to use the OSM
extract, use `http://posm.local/tiles/osm/{z}/{x}/{y}.png`.
