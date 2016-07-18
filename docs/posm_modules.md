# POSM Modules

POSM is intended as a modular system that can be configured for specific use-cases. It can be scaled
up or down depending on the needs of the environment and effort in which it is going to be involved
by adding or removing components.

## Base

As the LTS (long-term stability release) available upon POSM's inception, we chose to build on
Ubuntu 14.04 both for its long-term support and because it's relatively-well supported by open
source GIS tools that we use to glue components together.

For pragmatic reasons (which were valid at the start of the project, if not today), POSM is
primarily provisioned using a set of shell scripts (which can be used to preseed `debian-installer`
when installing from a USB stick) that download and configure each software component, typically
preferring the native packaging system for each underlying software platform (e.g. RubyGems with
`bundler`, `npm`, and `pip` with `virtualenv`) rather than creating Debian packages which would need
to be maintained separately for each.

An additional reason for using platform-native packaging systems is that they tend to do a better
job of isolating differing dependencies from other components (at the expense of disk space), which
is important for POSM, as we have little control over what our service dependencies require.

In some cases, system-wide versions are in use, e.g. Ruby, Python, and Node.js.

The individual "deploy" scripts in `posm-build` are generally intended to be runnable multiple times
without causing problems. Warnings and errors may be output, but they generally occur when a command
that can only produce a single effect is run more than once, e.g. user creation. However, in some
cases, data may be deleted, particularly when databases are (re-)configured.

## Modules

There are many pieces of software that are combined together to form POSM. This is a more granular
list of the components in use:

* [`admin`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/admin-deploy.sh) - [posm-admin](https://github.com/AmericanRedCross/posm-admin), POSM’s administrative interface and data provisioning tool
* [`bridge`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/bridge-deploy.sh) - bridged networking support--allows POSM devices to be used as conventional WiFi hotspots
* [`captive`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/captive-deploy.sh) - captive portal mode--prevents clients from accessing the internet while surfacing locally-available POSM services
* [`carto`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/carto-deploy.sh) - [posm-carto](https://github.com/AmericanRedCross/posm-carto), a TileMill style intended to highlight data present in OSM following the [Humanitarian Data Model](http://wiki.openstreetmap.org/wiki/Humanitarian_OSM_Tags)
* [`fieldpapers`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/fieldpapers-deploy.sh) - an offline-tuned [Field Papers](http://fieldpapers.org/) instance
* [`gis`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/gis-deploy.sh) - Miscellaneous command-line GIS tools (GDAL et al)
* [`mysql`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/mysql-deploy.sh) - MySQL server (required by Field Papers)
* [`nginx`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/nginx-deploy.sh) - HTTP server and reverse proxy
* [`nodejs`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/nodejs-deploy.sh) - Node.js runtime (required by other components)
* [`omk`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/omk-deploy.sh) - [OpenMapKit Server](https://github.com/AmericanRedCross/OpenMapKitServer)
* [`osm`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/osm-deploy.sh) - an [offline-tuned OpenStreetMap](https://github.com/AmericanRedCross/openstreetmap-website) instance
* [`postgis`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/postgis-deploy.sh) - PostgreSQL server with PostGIS (required by OSM)
* [`ruby`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/ruby-deploy.sh) - Ruby runtime (required by Field Papers and OSM)
* [`samba`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/samba-deploy.sh) - SMB fileshares
* [`tessera`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/tessera-deploy.sh) - Map tile server (renders posm-carto and MBTiles archives that have been provided)
* [`tl`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/tl-deploy.sh) - Command-line map tile Swiss Army knife
* [`wifi`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/scripts/wifi-deploy.sh) - access point support--allows POSM devices to be connected to wirelessly

## Creating a New Module

To create a new component, first decide on a name, say, “odm” (for OpenDroneMap). Next, clone
[posm-build](https://github.com/AmericanRedCross/posm-build) and create a new file called
`kickstart/scripts/odm-deploy.sh` that looks like:

```bash
#!/bin/sh

deploy_odm_ubuntu() {
  # install and configure OpenDroneMap
}

deploy odm
```

`odm` can now be deployed as part of a POSM system by running `kickstart/scripts/bootstrap.sh odm`
on a previously configured system.

To add `odm` to the default set of components installed on a new system, add it to the list in
[`kickstart/POSM_Server_USB.cfg`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/POSM_Server_USB.cfg).

## Settings

When provisioning a system, POSM settings are pulled from
[`etc/settings`](https://github.com/AmericanRedCross/posm-build/blob/master/kickstart/etc/settings)
and `etc/settings.local`. (`settings.local` is loaded and evaluated after `settings`, so overriding
any values that were used to generate derivative settings in `settings` requires also overriding
those derivative settings.) Settings are available as `bash` variables and can be interpolated into
[Handlebars](http://handlebarsjs.com/)-formatted configuration files (e.g. those in `etc/`) by using
the `expand` macro.

After installation (and updates), settings are written out as JSON to `/etc/posm.json`. [`jq`](https://stedolan.github.io/jq/) is the easiest way to load settings in application scripts that require them, e.g.:

```bash
posm_fqdn=$(jq .posm_fqdn /etc/posm.json)
```
