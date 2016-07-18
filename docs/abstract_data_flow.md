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
