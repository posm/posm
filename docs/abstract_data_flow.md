# Abstract Data Flow

## Philosophy

The [OpenStreetMap data model](http://wiki.openstreetmap.org/wiki/Elements) (nodes, ways, relations)
is POSM’s lingua franca. Many formats may be derived from it, but all are assumed to be lossy in
some way or another.

OSM XML and PBF formats are effectively interchangeable, but we prefer OSM PBF for its substantially
smaller file size (and roughly equivalent tool support).

Once data has been loaded into the system (from [export.posm.io](http://export.posm.io/) as an OSM
PBF), OSM’s API DB becomes the source of truth, as it retains the OSM data model and captures
historical editing activity. It can be relatively easily converted to OSM PBF (using Osmosis) in
order for other tools to create derivative forms (e.g. `osm2pgsql` updating a rendering database for
[posm-carto](https://github.com/AmericanRedCross/posm-carto) to use). This actually occurs
periodically as part of the backup process.

## Data Sources and Augmentations

Generally speaking, data passes through POSM in the following way:

1. Features are excerpted from OSM according to the active area of interest.
2. POSM Admin downloads a bundle and loads it into the databases that underly the OSM API and tile
   rendering.
3. A Field Papers atlas is created covering a task area.
3a. An MBTiles archive is created, covering the same area and containing tiles rendered locally
    according to POSM-carto.
3b. OSM XML is extracted from the local API DB.
3c. An OMK deployment is created containing the above artifacts.
4. OpenMapKit Android loads a deployment from OMK Server.
5a. Mappers go into the field and edit features present in the deployment / add new features with
    OMK Android.
5b. Mappers go into the field with Field Papers atlases and mark them up.
6. Mappers return and sync OpenDataKit with OMK Server.
7. OMK Server submits edits to the local OSM API.
8. Incorporate annotations made on Field Papers atlases using JOSM and/or iD.
9. Repeat from step 3.
10. QA edits
11. Merge with OpenStreetMap.org using the [POSM Replay Tool](https://github.com/americanredcross/posm-replay-tool).
