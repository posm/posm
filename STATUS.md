# End-to-End Status

This attempts to capture the current status of POSM's usability from end-to-end,
both in terms of software installed and configured and data made available.

## Legend

:computer: - automated

:hand: - manual

:sunny: - good to go

:cloud: - partially working

:bomb: - not working / unknown

## Misc. Cloud Services

Description | Status | Notes | Owner
----------- | ------ | ----- | -----
1. Data gatherer | :bomb: | **TBD**, HOT Export Tool? |

## Misc. Device Services

Description | Status | Notes | Owner
----------- | ------ | ----- | -----
1. Static iD assets | :bomb: | |
2. Field Papers | :cloud: | |
3. OMK Server | :cloud: | |
4. OSM API | :bomb: | macrocosm |
5. Captive Portal | :bomb: | **TBD** |
6. WiFi | :bomb: | `hostapd` |
7. 7. MBTiles server | :cloud: | `tessera` |
8. Tile renderer | :cloud: | `tessera` |

## Initial Data Gathering

Description | Automated | Status | Notes | Owner | Issues
----------- | --------- | ------ | ----- | ----- | ------
1. PBF Extract | :hand: | :bomb: | Can be done directly or using the [HOT Export Tool](http://export.hotosm.org/) with an arbitrary file format | |
1a. Overpass → XML | :hand: | :bomb: | | |
1b. XML → PBF conversion | :hand: | :bomb: | Osmosis | |
2. MBTiles creation | :hand: | :bomb: | [tl](https://github.com/mojodna/tl) or [mbtiles-generate](https://github.com/AmericanRedCross/mbtiles-generate) (which uses `tl` internally) | | [tl#15](https://github.com/mojodna/tl/issues/15)

### Outputs

Description | Status | Notes | Owner
----------- | ------ | ----- | -----
1. AOI data package | :bomb: | HTTP-accessible, contains PBF + MBTiles (styles TBD) for the AOI |

## Initial Device Data Provisioning

Description | Automated | Status | Notes | Owner
----------- | --------- | ------ | ----- | -----
1. Download data package | :hand: | :bomb: | Target path: **TBD** |
2. Extract MBTiles | :hand: | :cloud: | Target path: **TBD** |
3. Extract PBF | :hand: | :cloud: | Target path: **TBD** |
4. Populate APIDB<sup>1</sup> | :hand: | :bomb: | Osmosis |
5. Populate rendering DB | :hand: | :cloud: | `osm2pgsql` |
6. Restart tile server(s) | :hand: | :cloud: | |

1. To populate the APIDB (using scripts from [macrocosm](https://github.com/americanredcross/macrocosm)):

```bash
createdb macrocosm
psql -d macrocosm -f db-server/script/macrocosm-db.sql
osmosis \
  --read-pbf-fast delaware-latest.osm.pbf \
  --log-progress \
  --write-apidb database=macrocosm
```

### Outputs

Description | Status | Notes | Owner
----------- | ------ | -----| -----
1. HTTP-accessible tiles | :cloud: | From MBTiles archive(s) |
2. HTTP-accessible tiles | :cloud: | Live rendered |

## Field Papers

Description | Automated | Status | Notes | Owner
----------- | --------- | ------ | ----- | -----
1. Generate PDF | :hand: | :cloud: | fp-legacy / fp-tasks |
2. Download, print, and annotate | :hand: | :cloud: | |
3. Upload | :hand: | :bomb: | fp-legacy / fp-tasks |
4. Trace in iD | :hand: | :cloud: | static iD, fp-tiler |

### Outputs

Description | Status | Notes | Owner
----------- | ------ | ----- | -----
1. HTTP-accessible PDF | :cloud: | |
2. HTTP-accessible snapshot GeoTIFF | :bomb: | |
3. HTTP-accessible snapshot tiles | :bomb: | `fp-tiler` |

## OSM Editing

Description | Automated | Status | Notes | Owner
----------- | --------- | ------ | ----- | -----
1. Load data | :hand: | :bomb: | macrocosm |
2. Save changes | :hand: | :bomb: | macrocosm |

### Outputs

Description | Status | Notes | Owner
----------- | ------ | ----- | -----
1. Changesets | :bomb: | Inherently in the APIDB, but maybe we want files at this stage too |

## OpenMapKit

### Server

Description | Automated | Status | Notes | Owner
----------- | --------- | ------ | ----- | -----
1. Create form from XLS | :computer: | :sunny: | Working, bare-bones UI | Needs deep though regarding constraints. | 
2. Declare a deployment | :hand: | :cloud: | We have a very basic notion of a `manifest.json` for a job service | 
3. Serve a deployment to client | :computer: | :cloud: | The UI for this will be in the Android app, just starting that... | 
4. Serve forms to client | :computer: | :sunny: | Done. | 
5. Receive submissions and write to file system | :computer: | :sunny: | Done. | 
6. Aggregate / Export ODK submissions | :computer: | :cloud: | We don't have filters for that (not important now) | 
7. Export OSM Submissions | :computer: | :cloud: | API is done, filters and aggregates. Needs web UI |

### Android

Description | Automated | Status | Notes | Owner
----------- | --------- | ------ | ----- | -----
1. Get deployment | :hand: | :cloud: | Getting started creating UI and tasks to get deployment from server |
2. Get ODK Forms | :computer: | :sunny: | |
3. Integrate as plugin with ODK in survey workflow | :computer: | :sunny: | 
4. Select, tag, create POI | :computer: | :sunny: | 
5. Submit to server | :comptuer: | :sunny: |


## Sync ↓

Description | Automated | Status | Notes | Owner
----------- | --------- | ------ | ----- | -----
1. Regenerate AOI data package | :hand: | :bomb: |  
2. Download data package | :hand: | :bomb: | Target path: **TBD**  |
3. Extract PBF | :hand: | :bomb: | Target path: **TBD**  |
4. Extract MBTiles | :hand: | :cloud: | Target path: **TBD**  |

## Sync ↑

Description | Automated | Status | Notes | Owner
----------- | --------- | ------ | ----- | -----
1. Generate diff(s) from APIDB<sup>1</sup> | :hand: | :bomb: | Osmosis / `osmconvert` to go from DB → PBF and to generate `.osm` and/or `.osc` files.
2. Update diffs | :hand: | :bomb: | Staged for manual merging. Target: **TBD** |

1. Affected features in locally applied changesets need to be identified and renumbered if they did not previously exist.

## Periodic Tasks

Description | Automated | Status | Notes | Owner
----------- | --------- | ------ | ----- | -----
1. Sync APIDB → rendering DB| :hand: | :bomb: | Osmosis / `osm2pgsql` |
2. Flush tile cache | :hand: | :bomb: | Restart `tessera` |
