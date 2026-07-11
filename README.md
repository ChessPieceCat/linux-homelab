# Linux Homelab

A self-hosted Linux homelab designed to explore Linux administration, Docker, storage architecture, networking, and infrastructure engineering.

![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?logo=ubuntu&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=fff)
![Docker Compose](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)
![Nextcloud](https://img.shields.io/badge/Nextcloud-0082C9?logo=nextcloud&logoColor=white)
![Jellyfin](https://img.shields.io/badge/Jellyfin-00A4DC?logo=jellyfin&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-003545?logo=mariadb&logoColor=white)
![Tailscale](https://img.shields.io/badge/Tailscale-242424?logo=tailscale&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=white)

## Table of Contents

- [Overview](#overview)
- [System Architecture](#system-architecture)
- [Technology Stack](#technology-stack)
- [Objectives](#objectives)
- [Features](#features)
- [Design Decisions](#design-decisions)
- [Challenges and Solutions](#challenges-and-solutions)
- [Skills Demonstrated](#skills-demonstrated)
- [Lessons Learned](#lessons-learned)
- [Future Improvements](#future-improvements)

## Overview

A self-hosted Linux server built to explore infrastructure, system administration, and containerized application deployment.

This project documents the design, deployment, and maintenance of a homelab running on an HP EliteDesk 800 G6 Mini PC with Ubuntu Server and Docker Compose. The server hosts several services, including personal cloud storage, media streaming, service monitoring, and a centralized dashboard.

The project focuses on designing a maintainable system using persistent storage, containerized services, secure remote access, and documented infrastructure. Challenges involving Linux filesystems, Docker networking, and data persistence were solved to create a reliable environment.

The repository serves as both documentation for the homelab and a portfolio demonstrating practical experience with Linux, Docker, storage architecture, networking, and infrastructure design.

## What This Server Provides

This homelab currently provides:

- Personal media streaming through Jellyfin
- Private cloud storage using Nextcloud
- Service monitoring with Uptime Kuma
- Centralized service dashboard with Homepage
- Secure remote access using Tailscale

## System Architecture

<p align="center">
    <img src="assets/architecture/system-overview.svg" alt="Linux Homelab Architecture" width="900">
</p>

## Dashboard

Homepage provides a centralized dashboard for managing and accessing every service from a single interface.

<p align="center">
    <img src="assets/screenshots/homepage-dashboard.png"
         alt="Homepage Dashboard"
         width="900">
</p>

## Technology Stack

| Category         | Technologies                                       |
| ---------------- | -------------------------------------------------- |
| Operating System | Ubuntu Server 26.04 LTS                            |
| Hardware         | HP EliteDesk 800 G6 Mini PC                        |
| Containerization | Docker, Docker Compose                             |
| Applications     | Nextcloud, Jellyfin, Homepage, Uptime Kuma         |
| Database         | MariaDB                                            |
| Remote Access    | Tailscale                                          |
| Storage          | NVMe SSD, External NTFS HDD, ext4 filesystem image |
| Monitoring       | Uptime Kuma                                        |
| Version Control  | Git, GitHub

## Objectives

This homelab was built to develop practical experience with technologies commonly used in software engineering and infrastructure roles as well as to serve as an environment for learning and experimentation.

- Gain hands-on experience administering a Linux server
- Practice containerized application deployment using Docker and Docker Compose
- Design a self-hosted infrastructure composed of independent services
- Explore storage strategies and filesystem management
- Implement secure remote access without exposing services directly to the public Internet
- Develop experience troubleshooting deployment, networking, and permission issues
- Practice documenting infrastructure in a way that reflects professional engineering standards

The homelab continues to evolve as new services and automation capabilities are added.

## Features

The homelab consists of several independent services, each deployed as its own Docker Compose project.

### Homepage

Provides a centralized dashboard for accessing and monitoring all services from a single interface.

### Jellyfin

Media server for streaming movies, television shows, and other media stored on the external drive.

### Nextcloud

Private cloud storage platform for file synchronization and management. Data is stored on a dedicated ext4 filesystem mounted from an image file to provide Linux-compatible permissions while utilizing external NTFS storage.

### Uptime Kuma

Monitors service availability and provides health checks for the homelab environment.

## Design Decisions

### Containerized Services

Each application is deployed as an independent Docker Compose project rather than combining all services into a single Compose file. This isolates failures, simplifies maintenance, and allows each service to be updated, restarted, or modified separately.

### Persistent Data

Application data is stored outside of containers using bind mounts and Docker volumes. This ensures configuration, databases, and media remain intact when containers are recreated or updated.

### Storage Architecture

The server uses a two-tier storage strategy. The operating system and Docker runtime reside on the internal NVMe SSD for performance purposes, while media and application data are stored on a 2 TB external hard drive for long-term, inexpensive storage..

Nextcloud data is stored on an ext4 filesystem contained within an image file on the NTFS external drive. This provides native Linux filesystem permissions while avoiding the need to reformat the existing external drive and migrate previously stored data.

### Secure Remote Access

Remote administration is provided through Tailscale instead of exposing services directly to the public Internet.

### Service Monitoring

Uptime Kuma continuously monitors the availability of services, allowing issues to be detected quickly and verifying that services remain operational after changes or updates.

### Centralized Dashboard

Homepage provides a single interface for accessing all hosted services. This improves usability and reduces the need to remember individual service URLs and ports.

## Challenges and Solutions

### Challenge: Filesystem Compatibility

**Problem**

Nextcloud requires a Linux filesystem with support for POSIX permissions. The available external storage was formatted as NTFS, which does not provide the permission model expected by Nextcloud.

**Solution**

An ext4 filesystem was created inside a disk image stored on the external drive. The image is mounted during system startup and used as Nextcloud's data directory, providing Linux-compatible permissions while preserving the existing NTFS storage layout.

---

### Challenge: Persistent Container Data

**Problem**

Containers are designed to be ephemeral, meaning application data can be lost if it remains inside the container filesystem.

**Solution**

Persistent bind mounts and Docker volumes were configured for application data, databases, configuration files, and media libraries so containers can be recreated without data loss.

---

### Challenge: Service Organization

**Problem**

Managing multiple services can become difficult as the environment grows.

**Solution**

Each application was organized into its own Docker Compose project with dedicated configuration directories. This makes updates, troubleshooting, and future expansion easier.

## Skills Demonstrated

**Linux & System Administration**
- Linux administration
- Filesystem management
- Service management

**Containerization**
- Docker
- Docker Compose
- Container lifecycle management

**Infrastructure**
- Networking
- Persistent storage
- Monitoring
- Remote access

**Databases**
- MariaDB

**Professional Practices**
- Documentation
- Troubleshooting

## Lessons Learned

Building this homelab reinforced that designing reliable infrastructure involves far more than simply deploying applications. The most valuable lessons came from troubleshooting the issues encountered.

Key takeaways include:

- Linux filesystems and permissions have a major impact on how applications interact with data. Understanding POSIX permissions was essential when configuring storage for Nextcloud

- Containers should be treated as disposable. Persistent data belongs in bind mounts or Docker volumes to allow for the recreation of services without data loss.

- The separation of services into independent Docker Compose projects improves maintainability and reduces the impact of changes or failures in the system.

- Documenting infrastructure is as important as building it. Comprehensive and coherent documentation are essential to continuous maintenance and expansion. Extensive notes taken throughout development made it possible to produce this documentation and continue improving the homelab over time.

- Designing infrastructure requires balancing simplicity, security, and maintainability. The use of Tailscale for remote access and isolating services into independent projects reduced complexity and improved security and reliability.

## Future Improvements

- Automated backups

- Reverse proxy with HTTPS

- CI/CD deployment

- Centralized logging

- Authentication improvements

## Documentation

Additional technical documentation is organized by topic within the docs/ directory.

| Document | Description |
|----------|-------------|
| Architecture | System architecture and design decisions |
| Services | Configuration and deployment details |
| Storage | Storage layout and filesystem design |
| Networking | Docker networking and remote access |
| Troubleshooting | Common issues encountered and their solutions |
