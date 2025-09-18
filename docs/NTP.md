# NTP Server Setup Overview

This document describes the setup of a lab NTP server using Chrony in a Docker container on a Debian host

## Architecture
- **Chrony running inside a Docker container** to provide NTP services.  
- Container runs in **host network mode** and is granted `CAP_SYS_TIME` privileges to steer the host clock  
- Debian host time synchronization services (systemd-timesyncd or host Chrony) are disabled to prevent conflicts  
- Lab clients (pfSense, switches, APs, Windows desktops) point to this container for time synchronization  

## Installation and Configuration
- Created a Chrony configuration file (`chrony.conf`) with:
  - Upstream NTP pool servers for initial sync (`iburst` enabled)  
  - Internal network access restrictions (serve only lab VLANs)  
  - `local stratum 10` fallback for when upstream is unavailable  
  - Drift file persisted for time offsets, history, and client/server state  
  - `makestep 1.0 3` to correct large offsets quickly at startup; otherwise gradual slewing is used  
- Built a self-contained Docker image with Chrony installed and configuration copied in  
- Started the container with:
  - Host networking
  - `CAP_SYS_TIME` for clock adjustments
  - Mounted volumes for configuration and drift/state persistence
  - Automatic restart policy

## Network Integration
- Container IP added to pfSense general NTP setup so the router itself syncs to the lab NTP server  
- Switches and APs manually pointed to the container as their NTP server  

## Validation
- Verified NTP container is tracking upstream peers and maintaining stratum 2–4 accuracy  
- Confirmed Windows desktops, pfSense, switches, and APs synchronize successfully  
- Monitored connected clients via `chronyc clients` and `chronyc tracking` commands  
- Ensured only authorized VLANs can query the NTP server

## Notes / Lessons Learned
- Host system must not run competing NTP services  
- `CAP_SYS_TIME` is required for the container to adjust host time  
- Host network mode avoids bridging issues in LXC/Docker setups  
