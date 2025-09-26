# Set up for VLANs including subnets and DHCP
This document describes the initial configuration of VLANs and firewall rules for basic segmentation on my home network.

## Subnets
- Created sub interfaces and subnets for: 
- work stations 
- management devices (router, switch, etc.)
- guest WiFi SSID
- IoT WiFi SSID
- lab devices (servers and NUCs)
- Assigned each VLAN assigned its own subnet

## DHCP
- Enabled DHCP on each sub interface 
- Created reservations for critical devices
- Assigned dynamic address ranges for WiFi clients

## Firewall
- Created firewall rules enabling most inter VLAN traffic
- Restricted WiFi subnet from accessing any other subnet

## Switch config
- Set up VLANs on the switch including IDs and member ports with PVIDs attached
- Switch to router uplink is a trunk port with all VLANs tagged, WAP port has guest and IoT VLANs tagged, all other devices are -on access ports with their respective VLANs tagged
- Untagged native VLAN on all ports

## Validation
- Verified connectivity with pings and traceroute between all subnets 
- A few issues were caused by misconfigured ports and firewall rules

## Lessons Learned
- Always have a rollback plan before major changes: have out of band management and backup configs good to go 
