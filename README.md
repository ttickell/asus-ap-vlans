# VLANS Take Two for Auss Router/Access Points
Scripts to and maintain custom VLAN configurations on ASUS Access Points

## Purpose
The intent of this script is to configure an Asus router in access point mode to 
- Support setting tagged VLANs on any / all ethernet ports
- Support untagged traffic for any port being move to a specific VLAN
- Support moving traffic for any all Wireless SSIDs from the default network to one more more VLANs

Why?  

Doing this allows for a given wireless network (Guest, IOT, etc.t) to exist outside of your main network.  

This script is called "Take Two" because in addition to allowing for that it also turns the unit into an extension of the rest of your VLAN segmented network.  While writing the first iteration of this, it struck me that it would be much more useful if any of my VLANs were accessible on my Access Points.

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