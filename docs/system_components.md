# System Components

POSM is a collection of existing open source tools used elsewhere in the OpenStreetMap community,
glued together and lightly modified to better fit the Missing Maps use-case.

All server-side components assume Linux as a deployment environment (POSM itself is built atop
Ubuntu 14.04). User-facing components either have web interfaces or are cross-platform (e.g. JOSM,
the Java OpenStreetMap editor).

## Cloud

POSM incorporates a "cloud" component, used when provisioning devices with data to be deployed into
the field. Where possible, it uses existing instances of services. In some cases, those services
have needed to be augmented with additional functionality in order to gather appropriate data.

### OpenStreetMap

This is the OpenStreetMap.org that we all know and love. It is the source of all knowledge, and the
point from which offline data diverges (and is later returned). Within the Missing Maps context, it
is the "exchange database" where mapping efforts by different organizations converge and share.

Within the larger body of OSM conventions, the [Humanitarian OSM
Tags](http://wiki.openstreetmap.org/wiki/Humanitarian_OSM_Tags) stand out, as these are the
conventions that have been adopted by most humanitarian mapping efforts and generally apply to data
that's collected using POSM.

### OSM Export Tool

[HOT](https://hotosm.org/)'s [OSM Export Tool](http://export.hotosm.org/) provided an excellent
basis for producing data bundles to provision POSM with. We use a [forked
version](https://github.com/AmericanRedCross/osm-export-tool2) that exposes additional formats
(unfiltered OSM PBF, OSM XML) and can produce MBTiles archives. We've also contributed to [a set of
Ansible playbooks](https://github.com/americanredcross/osm-export-ops) to ease provisioning
development and production environments.

The OSM Export Tool uses Overpass behind the scenes to gather data for areas of interest.

## Device

The majority of services that facilitate collaboration run on the POSM devices. These devices are
typically CPU- and RAM-constrained in order to keep their cost and power drain low, so we try to
maintain as small a footprint as is feasible (while also avoiding re-implementing existing
functionality).

### OSM

POSM runs a significant chunk of the stack that powers OpenStreetMap.org on-device in order to
provide a consistent experience for users as well as to avoid needing to implement large swathes of
functionality from the ground up.

#### openstreetmap-website

We use a lightly customized version of openstreetmap-website (see the `posm` branch) to allow data
to be browsed and edited. The bulk of local modifications are to facilitate offline usage (disabling
functionality in certain places and reconfiguring map layers) and simplified editing (preventing
users from needing to log in). Additional modifications involve configuring the application to run
behind Nginx (using Puma).

#### CGImap

Like openstreetmap.org, we use CGImap to serve the bulk of API calls more efficiently than the Rails
application. We use it unmodified.

#### iD Editor

POSM maintains some minor local modifications to the iD editor. These consist of disabling
functionality that requires internet access (e.g. Nominatim (geocoding) and Taginfo (statistics +
info about how tags are used)) and configuring available background layers.

#### Misc. OSM tools

POSM also incorporates additional OSM and geospatial tooling that facilitate working with various data formats:

* [osmctools](https://gitlab.com/osm-c-tools/osmctools), e.g. `osmconvert`
* [Osmosis](http://wiki.openstreetmap.org/wiki/Osmosis)
* [osm2pgsql](http://wiki.openstreetmap.org/wiki/Osm2pgsql)
* [GDAL](http://www.gdal.org/)

### OpenMapKitServer

OpenMapKitServer is a Node.js-based OpenDataKit (ODK) Aggregate-compatible server with additional
functionality that allows OSM edits to be synchronized directly to an API endpoint. When used with
POSM, edits are submitted to the local API instance (typically [osm.posm.io](http://osm.posm.io/)).

### Field Papers

Field Papers runs unmodified, as necessary configuration hooks were built in during POSM
development. Notable changes relative to fieldpapers.org include the use of local disks (vs. S3) for
storage of PDFs, GeoTIFFs, and uploaded snapshots, a web hook configured to notify POSM Admin when
atlases have been created (producing an OMK "deployment" covering the same area), and exposure of an
SMB share that allows snapshots to be uploaded in bulk by copying them to a file share.

### POSM Admin

POSM comes with its own administrative interface. At present, the main purpose of POSM Admin is to
facilitate provisioning of data produced by the OSM Export Tool onto the device and ensure that all
of the appropriate databases and configuration files have been updated.

It can also be used to manage some of the network settings.

## Mobile

The POSM / Missing Maps workflow incorporates some Android applications that were built specifically
for it.

### OpenMapKit Android

[OpenMapKit](http://openmapkit.org/) (OMK) is an extension of OpenDataKit allowing users to create
professional quality mobile data collection surveys for field data collection.

OMK implements map-based surveys within the framework that OpenDataKit provides, allowing forms that
are created / managed using Excel to incorporate OSM features (provided to OMK as OSM XML) for
augmenting and addition.

### OpenDataKit

[OpenDataKit](https://opendatakit.org/) (ODK) is a free and open-source set of tools which help
organizations author, field, and manage mobile data collection solutions. For POSM's purposes, its
the framework within which OpenMapKit operates.

## Supporting Characters

### POSM-carto

[POSM-carto](https://github.com/AmericanRedCross/posm-carto) is a CartoCSS style specifically built
for POSM, intended to highlight Humanitarian Data Model (HDM) features that are typically collected
using the POSM/Missing Maps workflow. It is intended for use both as a background within OpenMapKit
(as the data shown matches the data being augmented) and as the print style for Field Papers.

POSM-carto includes a Natural Earth-powered worldwide locator map to help navigate to areas where
data has been imported.

TODO `service tessera restart` after loading new data
