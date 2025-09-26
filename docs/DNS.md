# DNS Setup Overview

This section documents the DNS setup my home lab, using Pi-hole and Unbound for recursive DNS with DNSSEC 

## Architecture
- Two separate LXC containers hosted on Proxmox:
  - **Unbound** → Recursive DNS resolver and DNSSEC validator.
  - **Pi-hole** → DNS filtering and caching for clients.
- Container interfaces rely on the host bridge interface for VLAN tagging and general networking.

## Installation and Configuration
- Created LXC containers based on Debian 12 images.
- Installed Unbound via `apt` and Pi-hole through a curl request to the official website
- Configured Unbound:
  - Root hints downloaded from official source for DNS root servers.
  - DNSSEC trust anchor generated and validated with `unbound-anchor`.
  - Security and performance features enabled (hide version, harden DNSSEC, prefetch, qname minimisation, cache TTLs).
- Configured Pi-hole:
  - Set Unbound as upstream DNS server.
  - Adjusted configuration to avoid duplicate trust anchors.
  - Set new credentials for the web UI admin access

## Network Integration
- Configured pfSense trusted, management, and lab interfaces to use Pi-hole as the primary DNS.
- Windows hosts configured to use dynamic DNS assignment from DHCP; Debian hosts validated to respect Pi-hole assignment.
- Cloudflare configured as a backup DNS.
- In the future I'll migrate the containers to a NUC, the current host draws too much power to run 24/7

## Validation
- Verified basic functionality with `dig` for standard domains.
- Verified DNSSEC validation using domains designed to pass/fail DNSSEC checks.
- Monitored Pi-hole logs and query statistics to confirm proper filtering and upstream resolution.
