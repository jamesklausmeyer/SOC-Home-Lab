# Active Directory Lab Environment

## Overview
- I set up an isolated AD lab for blue/red team simulations. The network is isolated from home LAN with only outbound HTTP/HTTPS allowed. All VMs run on a Proxmox node with a bridged network.

## VM Inventory
- Domain Controller:	2 vCPU	4GB	60GB	Windows Server 2022
- File Server:	2 vCPU	4–6GB	60GB	Windows Server 2022
- Workstation:	2 vCPU	4GB	60GB	Windows 10/11 Pro
- Splunk Server:	4 vCPU	12GB	100GB	Debian 12 / Ubuntu 22.04
- Atomic Red Attack Box:	2 vCPU	4GB	40GB	Kali Linux / Ubuntu + Atomic Red
- Print Server:	1 vCPU	2GB	20GB	Windows Server or Linux + CUPS

## Preparing Proxmox & Creating VMs
- Downloaded all ISO images for Windows Server, Windows 11, Debian, and Kali
- Created VMs with appropriate CPU, RAM, and storage
- Used SATA disks for Windows, SCSI for Linux

## Isolated Network & NAT
- Created a bridge network (vmbr1) in Proxmox for the lab
- Assigned static IP/DNS for Windows, Linux, and Kali clients
- Configured NAT on Proxmox to provide outbound internet access
- Enabled IP forwarding and applied firewall rules (default drop policy at datacenter level)

## Install Windows Server Roles & AD
- Configured Domain Controller and renamed to DC1
- Installed Active Directory Domain Services (ADDS)
- Created forest lab.local and configured DC as DNS server with upstream resolution
- Created OUs and users for servers and workstations
- Joined Windows clients to domain
- Configured Linux clients (Debian/Kali) with SSSD, realmd, Samba, and Kerberos for domain integration

## GPOs & Hardening
- Installed RSAT tools on a workstation for GUI-based AD management
- Created and applied GPOs for firewall rules, password policies, and account lockouts
- Set up helpdesk group with delegated permissions to reset passwords
- Enabled auditing for all events and monitored logs for verification
- Organized OUs and security groups using the AGDLP model

## Outcome
- Fully functional isolated AD lab for security testing
- Windows and Linux endpoints integrated into the domain
- Centralized management via GPOs and auditing enabled
