# Suricata IDS Integration (pfSense)

## Overview
- Integrated Suricata IDS/IPS into a pfSense lab environment to monitor network traffic and forward alerts to the ELK stack for analysis and visualization.

## Key Steps
- Installed Suricata via pfSense Package Manager
- Disabled hardware checksum offload and rebooted
- Enabled ETOpen and Emerging Threats rules
- Configured logging: Suricata alerts sent to pfSense firewall logs
- Forwarded logs directly to Logstash instead of Filebeat
- Updated Logstash pipeline to tag and index Suricata logs
- Verified Suricata alerts appear in Kibana

## Outcome
- Active intrusion detection monitoring on all lab interfaces
- Suricata logs successfully indexed in ELK
- Alerts and traffic patterns fully searchable and visualizable in Kibana
