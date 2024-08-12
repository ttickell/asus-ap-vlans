# VLANS Take Two for Auss Router/Access Points
Scripts to and maintain custom VLAN configurations on ASUS Access Points

## Purpose
The intent of this script is to configure an Asus router in access point mode to 
- Support setting tagged VLANs on any / all ethernet ports
- Support untagged traffic for any port being move to a specific VLAN
- Support moving traffic for any all Wireless SSIDs from the default network to one more more VLANs

Why?  

Doing this allows for a given wireless network (Guest, IOT, etc.t) to exist outside of your main network.  

This script is called "VLAN Take Two" because in addition to allowing for that it also turns the unit into an extension of the rest of your VLAN segmented network.  While writing the first iteration of this, it struck me that it would be much more useful if any of my VLANs were accessible on my Access Points, just so spare ports could be used to hardwire devices as needed.

## Knowledge Required
I don't actually know what I'm doing, but the knowledge for this much seems pretty low.

1. Know your existing network configuration 
   - if you care about adding VLANs, your probably have this much covered
2. Know the details of your ASUS unit
   - For instance, the interface allocations between my AC-86U (eth0-4 are physical, eth5 is 2.45ghz wifi, eth 6 is 5ghz) and my XT8 and my XT12 are all slightly different

The first pass script tried to insulate from that knowledge. It only cared about the Guest Wifi SSIDs and desired VLAN tag for them, the rest it could figure out on it's own.

This script can do that, too, provided you know which ports is your uplink to the rest of the network to run the tags on.  But, since it also changes arbitrary ports, you need to know what those ports are if you want that functionality. 

### If you didn't know something ...
If you screw something up, just reboot the unit.  I certainly have enough times, so far.

## Pre-Installation AP Configuration
Prerequisites for use are straight forward:
1. Have an Asus Router
2. Configure the Asus Router in Access Point Mode
3. Configure SSH access to the unit
4. Configure any Wifi Networks (primary and guest) as you would like
   - Do not use AI Mesh
5. Have the unit connected to something that understands VLAN Tagging _or_ have the unit connected to multiple discrete devices for differing networks with untagged ports
   - Granted, the later would be more useful in "router" mode, but nothing in this script manipulates the firewall setup to make that work
## A Locations and Configuration
No matter if you are useing Merlin or unmodified unit, the scripts and configuration will be stored under:

```
/jffs/local/etc/vlantt.d/ # Confifguratiojn files
/jffs/local/bin/vlan_tt   # The script itself
/jffs/local/bin/setup     # setup or check setup

```

The configuiration can have a few permutations, depending on what you want to achieve.  If you are just setting a specific guest network to be on a VLAN, then, in

```
/jffs/local/etc/vlantt.d/my_guest_vlan
```

You need to add the basics:  Which bridge, which port you have uplinked to the rest of your network, and the SSID of the network:

```
VLAN_ID=<vlan id>           # VLAN ID to use
PORTS_TAGGED="eth0"         # the uplink port is assuemd to have vlan tagging
BRIDGE_INT="br2"            # Which bridge to use - "brctl show" on the unit, and use the next se   quential bridge
WIFI_SSID="<network name">  # The name of the network
```
The full set of configuration options that may be in a file are:

- VLAN_ID: Which VLAN ID to used on the tagged PORTS
- PORTS_TAGGED: A common delimeted list of physical ports that may be tagged with VLAN_ID
- PORTS_UNTAGGED: a common delimted list of physical ports that will be added to the bridge  / made into untagged ports on VLAN_ID
- BRIDGE_INT : the bridge interface to be used.  Note it shoudl be "br#" where "#" is a numeric
- WIFI_SSID : the name of a wifi network to asscoiate

Of these, only VLAN_ID, PORTS_TAGGED, and BRIDGE_INT must be specified to take any action.  WIFI_SSID may be skipped if not wifi network should be associated with tthe VLAN, and PORTS_UNTAGGED may be skipped if there are no ports you wish to assciate.   However, if you skip both WIFI_SSID and PORTS_UNTAGGED, you will have a system that has access to a VLAN but you cannot actually attach a device to that VLAN via the unit.

You can have multiple files in directory, each with different configurion for differing permutations.  I have one access point where have four different configurations (they are the examples in this repository).
 - VLAN050-GUEST            # Settings for my guest network
 - VLAN051-IOT              # Settings for my IOT Network
 - VLAN051-IOT2             # Settings for a secondary SSID on them VLAN as my IOT Network
 - VLAN200-CAMERAS          # Settings to create an untagged port for a camera, no SSID

The VLAN051-IOT and VLAN051-IOT2 share the same VLAN_ID and BRIDGE_INT, but have differing PORTS_TAGGED and WIFI_SSIDs.  

The VLAN200-CAMERAS includes the option "PORTS_UNTAGGED" to associate a physical pport, but no WIFI_SSID.


## Installation on AP running Asuswrt-Merlin
### Initial Setup
#### Scripts and Config on /jffs
#### Updating Configuration
### Running on Reboots
### Running on GUI Changes
## Installation on stock Asus firmware
### Initial Setup
### Running on Reboots
### Running on GUI Changes
## Credits and Thanks
First, I stumbled upon this:

https://gist.github.com/Jimmy-Z/6120988090b9696c420385e7e42c64c4

It looks like Jimmy-Z has stopped usign a setup like this but, judging from all the subsquent forum posts I found, he started a wave. 

Weirdly, some seem to have ripped his work off, left enough to know where it came from, and claimed credit for that. I had a guy like that in work group in college once ... not cool, guys. Anyway, many thanks to Jimmy-Z.
