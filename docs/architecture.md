# System Architecture

## Purpose

This document describes the overall architecture of the Linux Homelab, including the host system, containerized services, storage layout, networking model, and the design principles that guided its implementation.

The goal of this document is to provide a high-level understanding of how the homelab is organized and how its components interact. More detailed configuration information for individual services is available in the corresponding documents within the `docs/` directory.

## System Overview

The homelab is built around a single Ubuntu Server host running Docker Engine and Docker Compose. Rather than installing applications directly on the operating system, each service is deployed as an independent containerized application.

The system currently provides:

* A centralized dashboard using Homepage.
* Personal media streaming through Jellyfin.
* Private cloud storage using Nextcloud.
* Service health monitoring with Uptime Kuma.
* Secure remote administration using Tailscale.

This modular architecture allows each service to be managed independently while sharing the same underlying host system and storage resources.

## Host System

The homelab runs on an HP EliteDesk 800 G6 Mini PC configured with:

| Component | Specification |
|-----------|---------------|
| CPU | Intel Core i5-10500 |
| Memory | 16 GB DDR4 |
| Operating System | Ubuntu Server 26.04 LTS |
| Container Runtime | Docker Engine |
| Orchestration | Docker Compose |

The operating system and container runtime are installed on the internal NVMe SSD to provide fast boot times and responsive application performance. Persistent application data and media libraries are stored separately on an external hard drive.