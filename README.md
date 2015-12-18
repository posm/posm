# POSM (Portable OpenStreetMap)

This is the GitHub home of POSM (Portable OpenStreetMap). POSM's goal is to
integrate best-of-breed tools from a variety of sources and developers on a
single device that can be deployed to the field alongside Red Cross staff and
volunteers and facilitate mapping efforts, particularly when internet access is
absent.

## Project-related Repositories

POSM is an aggregation of tools that are used in field enumeration and
digitizing efforts. This is a list of some of those tools and how they fit into
the overall effort.

### POSM

* [POSM](https://github.com/AmericanRedCross/posm) - You are here. This is the
  umbrella project for the POSM project and contains documentation and
  project-wide tasks.
* [osmposm](https://github.com/AmericanRedCross/osmposm) - Placeholder for an
  OSM-compatible API. Likely to be superseded by
  [macrocosm](https://github.com/developmentseed/macrocosm)
* [posm-api](https://github.com/AmericanRedCross/posm-api) - API to facilitate
  tasking of field enumerations (surveys + mapping).
* [posm-build](https://github.com/AmericanRedCross/posm-build) - Build scripts
  and configuration for software deployment.

### OpenMapKit

* [OpenMapKit](https://github.com/AmericanRedCross/OpenMapKit) - Umbrella
  project for [OpenMapKit](http://openmapkit.org/)
* [OpenMapKitAndroid](https://github.com/AmericanRedCross/OpenMapKitAndroid) -
  OpenMapKit Android application.
* [OpenMapKitServer](https://github.com/americanredcross/OpenMapkitServer) -
  Aggregation server for OMK surveys.
* [OpenMapKitWebsite](https://github.com/AmericanRedCross/OpenMapKitWebsite) -
  [openmapkit.org](http://openmapkit.org/)

### Field Papers

* [fieldpapers](https://github.com/fieldpapers/fieldpapers) - Umbrella project
  for [Field Papers](http://fieldpapers.org/). Contains documentation and issues.
* [fp-legacy](https://github.com/fieldpapers/fp-legacy) - (Now-unused) PHP code
  for the website, Python atlas creation + snapshot processing tools
* [fp-tasks](https://github.com/fieldpapers/fp-tasks) - Node.js-based HTTP task
  runner for generating atlases and processing snapshots.
* [fp-tiler](https://github.com/fieldpapers/fp-tiler) -
  [tessera](https://github.com/mojodna/tessera)-based GeoTIFF map tiler for
  processed snapshots.
* [fp-web](https://github.com/fieldpapers/fp-web) - Rails-based Field Papers
  website.
* [josm-fieldpapers](https://github.com/fieldpapers/josm-fieldpapers) -
  [JOSM](https://josm.openstreetmap.de/) plugin for Field Papers.
* [Transifex](https://www.transifex.com/fieldpapers/) - Field Papers
  translations.

### Ancillary

* [OpenDataKit](https://opendatakit.org/) - Mobile data collection for Android.
* [OpenStreetMap](http://openstreetmap.org/) - The one, the only, OpenStreetMap.
* [tessera](https://github.com/mojodna/tessera) -
  [tilelive](https://github.com/mapbox/tilelive)-based tile server. Can be used
  for serving static tiles (from MBTiles archives or elsewhere) and for
  rendering new data.
* [tl](https://github.com/mojodna/tl) -
  [tilelive](https://github.com/mapbox/tilelive)-based tile utility belt. Used
  for creating MBTiles archives from both local and remote sources.
* ...
