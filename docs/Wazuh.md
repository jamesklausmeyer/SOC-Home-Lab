# Wazuh HIDS Integration

## Overview
- Integrated Wazuh as a host-based intrusion detection system (HIDS) in the lab environment to for centralized monitoring of endpoints
- The Wazuh stack will replace the previously built ELK stack as a centralized log destination for the network

## Wazuh Manager Deployment
- Deployed the prebuilt Wazuh VM image with ~4 CPU cores, 8+ GB RAM, and 60+ GB storage
- Converted disk from VMDK → QCOW2 for Proxmox compatibility
- Configured VM with UEFI/OVMF BIOS and Q35 machine type
- Booted VM and accessed the Wazuh web dashboard
- Updated default OS and web admin passwords

## Agent Deployment
- Installed Wazuh agent on Windows endpoints via MSI installer
- Installed Wazuh agent on Proxmox/Linux endpoints via Debian package
- Enabled and started agents to report to the Wazuh manager

## Outcome
- Centralized HIDS monitoring across lab endpoints
- Security events from agents are ingested into Wazuh dashboard 
- Provides continuous visibility into system-level security alerts
