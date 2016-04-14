![POSM](https://raw.githubusercontent.com/americanredcross/posm/master/mediakit/logo-full.png "Portable OpenStreetMap")

This is the GitHub home of POSM (Portable OpenStreetMap). POSM's goal is to
integrate best-of-breed tools from a variety of sources and developers on a
single device that can be deployed to the field alongside Red Cross staff and
volunteers and facilitate mapping efforts, particularly when internet access is
absent.

## More Information

For more information, check the following sources:

* [SpatialDev's introductory blog post](http://news.spatialdev.com/portable-open-street-map/)
* [Stamen's introductory blog post](https://hi.stamen.com/taking-open-street-map-into-the-field-a07265a08f19)
* [Dale's announcement mailing list post](https://lists.openstreetmap.org/pipermail/talk/2015-December/075250.html)

## Glossary

This is intended to provide a working definition of various terms used
throughout POSM.

* **AOI** - Area of Interest.
* **Atlas** (Field Papers) - Identified by a Field Papers URL and represented as
  a QR code.
* **Deployment** - The sending of staff to a _deployment AOI_. Alternately, the
  provisioning of associated artifacts necessary to facilitate enumeration to a
  server accompanying staff into the field.
* **Deployment AOI** - A region that staff are deployed to. Typically
  corresponds to imagery / logistical requests.
* **Digitization** - Tracing / transcribing features present on a _page_ into a
  GIS system (e.g. JOSM or iD).
* **Extent** - Bounding box coordinates describing an _AOI_.
* **Field Enumeration** - Collection of information about geographic features
  in the field.
* **Form** (OpenMapKit) - Set of questions used during a field survey. Generally
  unique to a _deployment AOI_.
* **Page** (Field Papers) - Single sheet of paper. Part of an _atlas_;
  identified using grid coordinates (e.g. A1, F6, ...). Corresponds to an
  individual _survey area_. Identified by a Field Papers URL and represented as
  a QR code.
* **Provision** - Artifacts associated with a _task area_.
* **Snapshot** (Field Papers) - Identified by a Field Papers URL.
* **Survey** (OpenMapKit) - ???
* **Survey Area** - A focused area within a _task area_. Should be sized
  appropriately that multiple _survey areas_ can be covered by the same time in
  a single day. Corresponds to a single _page_ within an _atlas_.
* **Task Area** - A focused area within the _deployment AOI_. Corresponds to a
  set of _pages_ within an _atlas_.
* **Response** (OpenMapKit) - Collected metadata for an individual feature
  within a _survey area_.

## Hardware

Our initial hardware target is an Intel NUC w/ a 256GB SSD and 8GB of RAM.
Initial measurements show it drawing less than 10W. It can also operate on a
range of voltages between 12-19V. The spec'd components (below) run about USD
~$300.

* [NUC Kit - BOXNUC5PPYH](http://smile.amazon.com/gp/product/B00XPVQHDU) - 2.6
  GHz quad core Pentium (N3700, 6W TDP). Includes a basic case.
* [Samsung 850 EVO 250GB](http://smile.amazon.com/gp/product/B00OAJ412U)
* [8GB 1600MHz DDR3L CL9 SODIMM](http://smile.amazon.com/gp/product/B00KQCOTCM)
* [Wireless N Dual Band + Bluetooth 4.0 M.2 NGFF Combo Card](https://www.thinkpenguin.com/gnu-linux/wireless-n-dual-band-bluetooth-40-m2-ngff-combo-card) -
  needed to support AP mode with > 14 clients. Replace the stock wireless card
  with this.

## Project-related Repositories

POSM is an aggregation of tools that are used in field enumeration and
digitizing efforts. This is a list of some of those tools and how they fit into
the overall effort.

### POSM

* [POSM](https://github.com/AmericanRedCross/posm) - You are here. This is the
  umbrella project for the POSM project and contains documentation and
  project-wide tasks.
* [posm-admin](https://github.com/AmericanRedCross/posm-admin) - Simple admin
  interface for posm configuration & management.
* [posm-build](https://github.com/AmericanRedCross/posm-build) - Build scripts
  and configuration for software deployment.
* [osm-export-tool2](https://github.com/AmericanRedCross/osm-export-tool2/tree/posm) - A 
fork of the HOT Export tool used to create POSM data bundles.

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
* [pyXforms](https://github.com/uw-ictd/pyxform)-for converting Excel based surveys.
* ...
