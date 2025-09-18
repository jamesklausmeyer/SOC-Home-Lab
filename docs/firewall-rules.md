# Firewall Rules Overview

## Purpose
These rules are designed to allow the bare minimum of traffic to pass through while maintaining intended inter VLAN communications and internet access

## Rule Categories
- **Inter-VLAN Traffic**
  - Trusted and management VLAN devices can access everything
  - Lab VLAN can access management devices for ICMP traffic and nothing else
  - Guest/IoT VLANs have outbound WAN access and nothing else

- **Management VLAN**
  - Accessible only from trusted VLANs and lab for ICMP/core services

- **Lab VLAN**
  - Allows stateful communication for core services (DNS, NTP, etc.) to trusted and management
  - Outbound WAN allowed to access package managers and for iterative DNS

## Implementation Notes
- Rules are applied in order of priority to enforce least-privilege access.
- An implicit deny rule is at the bottom of every interface's rule list
- Logging is enabled on each implicit deny rule

## Validation
- Used built in pfSense diagnostics to send pings between interfaces to ensure no intended inter VLAN traffic was permitted
- Ran packet captures and observed performance of the rules

