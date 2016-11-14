# Local OSM Replication

## Diff Generation

POSM uses [Osmosis' replication features](https://wiki.openstreetmap.org/wiki/Osmosis/Replication)
to write minutely diffs to `/opt/data/osm/replication/minute`.

Initialization is done at [build
time](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/osm-deploy.sh#L6-L22) and
[when extracts are
loaded](https://github.com/AmericanRedCross/posm-admin/blob/master/scripts/osm_api-db-populate.sh#L35-L44).

Minutely diffs are generated via [the `osm` user's
crontab](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/etc/osm.crontab).

## `osm2pgsql` Diff Consumption

[POSM Carto](https://github.com/AmericanRedCross/posm-carto) uses
[`osm2pgsql`](https://github.com/openstreetmap/osm2pgsql) to populate its rendering database.

The rendering database is initially populated [while loading
extracts](https://github.com/AmericanRedCross/posm-admin/blob/master/scripts/gis_render-db-pbf2render.sh#L23-L32).

Replication state (creation of `state.txt`) is reset [at the same
time](https://github.com/AmericanRedCross/posm-admin/blob/master/scripts/gis_render-db-pbf2render.sh#L34).
Updates are applied minutely via [the `gis` user's
crontab](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/etc/gis.crontab),
using
[`apply-updates.sh`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/etc/apply-updates.sh),
which aggregates diffs from `/opt/data/osm/replication/minute` into OsmChange (`.osc`) files that
`osm2pgsql` applies to existing data (using `--append`).

Lists of tiles to expire (at z22) are written to `/opt/data/osm/expiry/<timestamp>.txt`. These are
intended for generating `PURGE` requests against local caches (if necessary).
