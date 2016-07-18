# Practical Data Flow

## 1. Feature Excerpting

The OSM Export Tool at [export.posm.io](http://export.posm.io/) is used to select an area of
interest and produce a "POSM bundle" containing an OSM PBF (sourced from the [Overpass
API](http://overpass-api.de/)) and (optionally) MBTiles archives from publicly-available tile
servers.

## 2. Data Provisioning

POSM Admin is used to download the POSM bundle from a publicly-available URL and load it into the
following local databases:

* API DB (powering the local OSM API + feature browsing)
* Rendering DB (powering [POSM-carto](https://github.com/AmericanRedCross/posm-carto))

## 3. Create a Field Papers atlas

Use the Field Papers interface to create an atlas covering a task area. After Field Papers has
finished creating the PDF, it will notify POSM Admin (using a web hook configured as an environment
variable).

When triggered, POSM Admin will use Osmosis to extract specific features within the task area from
the API DB and write them out as OSM XML (for download by OMK Android). It also runs a tile
generation job (using `tl` and POSM-carto) to generate an MBTiles archive that OMK Android will use
as the basemap when editing.

## 4. Load a Deployment into OMK Android

[screenshot goes here]

## 5. Mappers Gonna Map

Send mappers into the field and have them improve the data that they were sent out with.

## 6. Sync Android Devices with OMK Server

Synchronizing data modified using OMK Android involves using OpenDataKit to synchronize with an ODK
Aggregate-compatible server, i.e. OMK Server. This includes survey responses and modified OSM XML.

[screenshot goes here]

## 7. OMK Server Submits Edits to the Local OSM API

Each feature edit made using OMK Android is submitting to the local OSM API as an individual
changeset. This allows the data to be used for subsequent field efforts and submits in sufficiently
granular fashion that merging with OpenStreetMap.org will be less painful (due to smaller individual
changes).

## 8. Incorporate Field Papers Atlas Annotations

Scan or take photos of annotated Field Papers atlases and upload them as snapshots. This can be done
in bulk by copying them to the `\\posm\fieldpapers` SMB share (or individuals can be uploaded using
the web UI).

JOSM (using the Field Papers plugin) and iD (using the snapshot as a background image) can be used
to make local edits using atlas annotations for reference. These edits should then be grouped
logically into changesets.

## 9. Repeat From Step 3

New atlases will incorporate edits made during the previous cycle and will trigger OMK deployments
containing similarly-updated data.

## 10. QA Edits

As mappers complete their edits, local staff should QA their edits in preparation for submitting the
data back to OpenStreetMap.org and using it for other purposes.

## 11. Merge with OpenStreetMap.org

Use the [POSM Replay Tool](https://github.com/americanredcross/posm-replay-tool) to reconcile merge
conflicts and generate a series of changesets to be pushed up to OpenStreetMap.org, effectively
re-synchronizing OSM with data that was modified while in the field.
