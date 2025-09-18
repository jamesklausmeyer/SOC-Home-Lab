# VPN Setup Overview

This document describes the setup of a WireGuard VPN for secure remote access to my home lab

## Architecture
- WireGuard installed on the pfSense router  
- VPN subnet used exclusively for tunnel traffic  
- Laptop configured as the only peer  
- VPN allows access to Management and Lab VLANs while maintaining segmentation from Guest/IoT networks

## Installation and Configuration
- Installed WireGuard package on pfSense  
- Created a VPN tunnel with a dedicated subnet  
- Generated key pairs for pfSense and remote peers  
- Configured pre-shared keys for additional security  
- Added the laptop as a peer with appropriate routing for Management and Lab subnets  
- Configured laptop with WireGuard client, specifying:
  - Private key
  - Pre-shared key
  - VPN address
  - DNS pointing to lab DNS server
  - Allowed IPs for tunneled traffic
- Created WAN firewall rule to allow incoming VPN traffic  
- Added firewall rules to permit VPN traffic to Management and Lab subnets  
- Adjusted NAT mode to hybrid and created outbound NAT rules to translate local VPN traffic to a public address

## Troubleshooting / Lessons Learned
- Initial misconfiguration of peer subnet CIDR on pfSense caused connectivity issues  
- Firewall rules had protocol restrictions that were too strict, switching to “any” resolved access problems.  
- Ensured resolvconf installed on client to handle DNS correctly over VPN

## Validation
- Successfully established tunnel from laptop to home lab over public internet  
- Confirmed access to Management and Lab VLANs while maintaining restrictions for other networks  
- Tested DNS resolution and ping/traceroute from the VPN client to lab resources
