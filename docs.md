# Portable OpenStreetMap (POSM)

## Genesis

[Nick should explain where the idea came from.]

American Red Cross / Missing Maps workflow involves both OpenMapKit and Field Papers. Wanted to join
them together in something that could be taken into the field and used with limited or no internet
access available.

## Components

POSM is built on top of Ubuntu 14.04 LTS and incorporates the following components:

* osm-export-tool2
* openstreetmap-website
* OpenMapKitServer
* Field Papers
* posm-carto
* posm-admin

### Ubuntu 14.04

As the LTS (long-term stability release) available upon POSM's inception, we chose to build on
Ubuntu 14.04 both for its long-term support and because it's relatively-well supported by open
source GIS tools that we use to glue components together.

TODO move to a posm-build section

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

### osm-export-tool2

[HOT](https://hotosm.org/)'s [OSM Export Tool](http://export.hotosm.org/) provided an excellent
basis for producing data bundles to provision POSM with. We use a [forked
version](https://github.com/AmericanRedCross/osm-export-tool2) that exposes additional formats
(unfiltered OSM PBF, OSM XML) and can produce MBTiles archives. We've also contributed to [a set of
Ansible playbooks](https://github.com/americanredcross/osm-export-ops) to ease provisioning
development and production environments.

### openstreetmap-website

We use a lightly customized version of openstreetmap-website (see the `posm` branch) to allow data
to be browsed and edited. The bulk of local modifications are to facilitate offline usage (disabling
functionality in certain places and reconfiguring map layers) and simplified editing (preventing
users from needing to log in). Additional modifications involve configuring the application to run
behind Nginx (using Puma).

#### CGImap

Like openstreetmap.org, we use CGImap to serve the bulk of API calls. We use it unmodified.

#### iD Editor

POSM maintains some minor local modifications to the iD editor. These consist of disabling
functionality that requires internet access (e.g. Nominatim (geocoding) and Taginfo (statistics +
info about how tags are used)) and configuring available background layers.

### OpenMapKitServer

OpenMapKitServer is a Node.js-based OpenDataKit (ODK) Aggregate-compatible server with additition
functionality that allows OSM edits to be synchronized directly to an API endpoint. When used with
POSM, edits are submitted to the local API instance (typically [osm.posm.io](http://osm.posm.io/)).

[Nick to write more here]

### Field Papers

Field Papers runs unmodified, as necessary configuration hooks were built in during POSM
development. Notable changes relative to fieldpapers.org include the use of local disks (vs. S3) for
storage of PDFs, GeoTIFFs, and uploaded snapshots, a web hook configured to notify POSM admin (or
OMK Server?) when atlases have been created, and exposure of an SMB share that allows snapshots to
be uploaded in bulk by copying them to a fileshare.

TODO create a Field Papers section in "Use" / "Operation"

TODO elaborate on the effect of the web hooks

TODO elaborate on how snapshots can be uploaded in bulk

TODO highlight CSV grids

### posm-carto

TODO discuss HDM support

TODO discuss worldwide locator map

TODO mention slowness

TODO `service tessera restart` after loading new data

### posm-admin

## Installation

### Configuration

### Something like Installation but not repetitive

## Use

### Data Deployment

### Typical Workflow

#### OMK Forms

#### FP Atlas Creation

#### OMK Submission

#### FP Snapshot Creation

#### Data Editing and QA

TODO JOSM API endpoint configuration

#### Merging Data with OpenStreetMap.org

## POSM Operation

POSM has a single account that can be logged into: `root`. The default password is `posm`. You can
either log in directly through a console (when a display is connected) or over SSH. When connected
to the POSM's WiFi, use `ssh root@posm.io`.

### Data

#### APIDB

TODO openstreetmap-website to create

TODO Osmosis to load

TODO username change gotchas

#### Rendering DB

TODO paired `gis` databases

TODO `osm2pgsql` to load

#### AOIs

TODO PBFs

#### OMK

#### Field Papers

TODO atlas PDFs

TODO snapshot GeoTIFFs

TODO raw snapshots

TODO MySQL database

### Periodic Tasks

### Backups

TODO what/where/when

### Networking

POSM is configured to be a WiFi Access Point (AP), allowing laptops and phones to connect to it
directly even if no other network is available. It supports 2 modes: "captive portal" and "bridged".

In "captive portal" mode, after clients connect, all HTTP traffic will be intercepted and redirected
to `posm.io`, which is POSM's landing page. Requests to web sites over HTTPS will fail. Only access
to POSM services (`posm.io` and `osm.posm.io`) is allowed in this mode. If the WAN port is plugged
into an active network, the POSM itself will have network access but clients will not.

In "bridged" mode, clients will not be automatically redirected and will instead have internet
access, forwarded through whichever WAN device is connected (see "WAN" below).

Switching between these modes can be done using POSM Admin (TODO can it?). Alternately

#### WiFi

When using POSM with one of the recommended NUCs, the pre-installed wireless cards can only be used
in 802.11a/g/b mode (this is the default when setting up a new device). Additionally, these cards
are only able to support 15 clients (including the POSM device itself). If you need to support more
concurrent clients, we recommend replacing the wireless NIC with TODO, which supports more clients
(we don't know the actual limit, but it's at least 25) **and** supports 802.11n mode (faster).

If you replace the card on an existing device, you'll need to TODO clear out the `udev` rules to
allow Linux to identify the new card. You'll also want to re-bootstrap `wifi` after changing
`posm_wifi_80211n` to `1` in order to enable 802.11n mode.

An additional option which we don't have much experience with yet is to purchase something like a
Ubiquiti UAP-AC-LITE and plug it into the LAN interface (which doesn't actually exist on the NUCs,
since they only have a single NIC which is configured as a WAN port). This will offload the
management of wireless clients (but not DNS or DHCP), allowing more concurrent clients to connect
(127 per radio; some UAPs include multiple radios). This approach is also assumed to increase the
WiFi range.

#### WAN

When in "bridge" mode, POSM is configured to pass traffic to non-`wlan` interfaces. Practically
speaking, this means that you can plug in an ethernet cable to the WAN port, POSM will obtain an IP
address and DNS servers using DHCP, and will configure `iptables` to forward traffic.

When used with an Android device that has USB tethering enabled, plugging in a USB cable will create
an additional network interface and POSM will attempt to forward traffic to it.

Behavior with both a wired network and a tethered Android device is unknown; it's suspected that the
interface plugged in last will be used to forward traffic. Unplugging either will likely remove all
forwarding rules.

For more information about this, see the hotplug scripts in TODO.

Some services on the POSM device are available via the WAN port, most usefully SSH. DNS, DHCP, and
SMB are limited to the LAN. POSM uses `avahi-daemon` to broadcast its presence on the WAN, meaning
that `ssh root@posm.local` may be used to connect even if its IP address is unknown.

POSM has also been configured to support link-local connections (TODO WTF is this actually called?
these are the 169.whatever IPS), allowing an ethernet cable to be plugged into both the WAN port and
a client. Both devices will auto-assign themselves IP addresses in the same rang. This, combined
with mDNS broadcasts, allows direct connections. Beyond facilitating debugging without a display,
this is the fastest way to copy data to or from the device.

### Available Utilities

* Osmosis
* osm2pgsql
* osmconvert
* osmupdate
* GDAL/OGR
* tl (w/ misc. tilelive modules)
