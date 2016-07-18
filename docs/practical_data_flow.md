# Practical Data Flow

## 1. Feature Excerpting

The OSM Export Tool at [export.posm.io](http://export.posm.io/) is used to select an area of
interest and produce a "POSM bundle" containing an OSM PBF (sources from the [Overpass
API](http://overpass-api.de/)) and (optionally) MBTiles archives from publicly-available tile
servers.

## 2. Data Provisioning

POSM Admin is used to download the POSM bundle from a publicly-available URL and load it into the following local databases:

* API DB (powering the local OSM API + feature browsing)
* Rendering DB (powering [POSM-carto](https://github.com/AmericanRedCross/posm-carto))

## 3. Create a Field Papers atlas

Use the Field Papers interface to create an atlas covering a task area. After Field Papers has finished creating the PDF, it will notify POSM Admin (using a web hook configured as an environment variable).

When triggered, POSM Admin will use Osmosis to extract specific features within the task area from the API DB and write them out as OSM XML (for download by OMK Android). It also runs a tile generation job (using `tl` and POSM-carto) to generate an MBTiles archive that OMK Android will use as the basemap when editing.

## 4. Load a Deployment into OMK Android

[screenshot goes here]

## 5. Mappers Gonna Map

Send mappers into the field and have them improve the data that they were sent out with.
