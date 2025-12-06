Debian 13 VM + Dockerized ELK Stack (Elasticsearch, Logstash, Kibana)

A lightweight, production-style logging pipeline built on a Debian VM.

## Overview

This project sets up a Debian 13 virtual machine running a fully containerized ELK stack (Elasticsearch, Logstash, Kibana). The VM acts as a centralized logging server capable of receiving logs over Beats, TCP, or traditional syslog (TCP/UDP), parsing them, and indexing them into Elasticsearch for visualization in Kibana.

The goal of the build was to recreate a realistic, modular, security-aligned logging pipeline similar to what you’d manage in a SOC or cloud environment.

## VM Deployment (Debian 13 Base System)

The system runs on a Proxmox VM with:

4 CPU cores

16 GB RAM

100 GB storage

Bridged networking

Static DHCP lease (192.168.20.9)

After installation, the VM was brought up to date and prepared with fundamental packages required for Docker and containerized services.

## Docker & Container Runtime Setup

Docker and Docker Compose were installed to provide:

Isolated service deployment

Easy portability of configurations

Ability to run Elasticsearch, Logstash, and Kibana as independent components

Persistent data volumes mapped into the VM filesystem

A non-root user was added to the Docker group for operational convenience.

## Elasticsearch: Single-Node Logging Backend

Elasticsearch was deployed as the foundational datastore.

Key steps included:

Configuration Highlights

Single-node mode (no clustering needed)

Memory heap tuning to match available VM resources

Data persistence using a host-mounted directory

Required system parameters updated (e.g., vm.max_map_count)

Security features disabled initially for ease of testing (later re-enabled)

Outcome

Elasticsearch became reachable via http://<VM_IP>:9200, ready to receive log data from Logstash.

## Logstash: Central Log Ingestion & Parsing Engine

Logstash was configured to act as the primary log collection and parsing component.

Pipelines Included

Beats → Elasticsearch pipeline
Designed for Filebeat or Winlogbeat ingestion.

TCP testing pipeline
Allowed sending simple messages over TCP for pipeline validation.

Syslog pipeline (TCP/UDP 514)
Added later to support devices or services that use traditional syslog formats.

Parsing & Filtering

Grok patterns for syslog (SYSLOGBASE)

Optional JSON parsing for Filebeat events

Timestamp normalization

Tagging, type assignment, and field enrichment

Architecture Enhancements

A dedicated Docker network (docker_elk) was created so all containers could communicate cleanly over internal hostnames.

## Kibana: Visualization and Analysis

Kibana was deployed to provide:

Dashboards

Index pattern management

Real-time log inspection

Dev Tools for querying logs with Elasticsearch DSL

Kibana connected to Elasticsearch through the shared Docker network and was exposed to the LAN on port 5601.

## Filebeat / Syslog Integration

During testing, some parsing and formatting issues surfaced with Filebeat, leading to:

Switching Filebeat output to Logstash over syslog-compatible TCP/UDP instead of native Beats

Adding codecs and filters in Logstash to properly handle plaintext and JSON combinations

Updating Logstash pipelines to handle mixed log types (Windows, Linux, network devices, etc.)

This produced a flexible ingestion pipeline that can accept:

Beats (native)

Syslog (TCP and UDP)

Plain-text TCP streams

JSON events

## Docker Compose Consolidation

After validating all individual components, the services were consolidated into a single docker-compose.yml stack, including:

Elasticsearch service with persistent volumes

Logstash with pipeline mounting and port exposure

Kibana with persistent data storage

Shared elk Docker network

Health checks and authentication were later added to Elasticsearch, requiring updates to Logstash pipeline credentials.

Once finalized, the stack could be brought online using a single command.

## Final Architecture

VM (Debian 13)
→ Docker Engine
→ Containers:

Elasticsearch (storage backend, port 9200)

Logstash (ingestion, parsing, SYSLOG/Beats/TCP, ports 5044/5000/514/9600)

Kibana (web UI, port 5601)

External sources such as Filebeat, Windows hosts, and network devices were successfully tested and ingested into the pipeline.
