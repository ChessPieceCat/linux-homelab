# Deployment Guide

## Purpose

This document describes the process used to deploy the Linux Homelab from a clean Ubuntu Server installation. It serves as both deployment documentation and a disaster recovery guide, allowing the homelab to be recreated if the server hardware or operating system must be replaced.

---

## Prerequisites

Before beginning deployment, ensure the following requirements are met:

### Hardware

* HP EliteDesk 800 G6 Mini PC (or equivalent)
* Internal NVMe SSD
* External 2 TB HDD
* Stable network connection

### Software

* Ubuntu Server 26.04 LTS
* Git
* Docker Engine
* Docker Compose
* Tailscale

---

## Directory Structure

The homelab organizes each service into its own project directory.

Example:

```text
/home/user/
├── homepage/
├── jellyfin/
├── nextcloud/
└── uptime-kuma/
```

Each directory contains its own Docker Compose file and service-specific configuration.

---

## Initial Server Configuration

After installing Ubuntu Server:

1. Update the system packages.
2. Install Git.
3. Install Docker Engine.
4. Install Docker Compose.
5. Install and configure Tailscale.
6. Mount the external storage device.
7. Mount the Nextcloud ext4 filesystem image.
8. Verify that all mount points are available.

---

## Deploying Services

Each service can be deployed independently using Docker Compose.

General deployment process:

1. Navigate to the service directory.
2. Review the Compose configuration.
3. Configure environment variables if required.
4. Start the service using Docker Compose.
5. Verify that the container is running correctly.

Because each service is isolated, deployment failures are limited to the affected application.

---

## Verification

After deployment, verify that:

* All Docker containers are running.
* Persistent storage is mounted correctly.
* Homepage displays all configured services.
* Jellyfin can access the media library.
* Nextcloud can read and write data.
* Uptime Kuma reports healthy service status.
* Remote access through Tailscale functions correctly.

---

## Updating Services

Routine updates consist of:

1. Pulling updated container images.
2. Recreating containers using Docker Compose.
3. Confirming that persistent data remains intact.
4. Verifying service health using Uptime Kuma.

Because application data is stored outside of containers, updates do not affect user data or configuration.

---

## Disaster Recovery

If the operating system must be reinstalled:

1. Install Ubuntu Server.
2. Restore Docker and Docker Compose.
3. Restore the project directories.
4. Mount the external storage device.
5. Mount the Nextcloud filesystem image.
6. Deploy each service using Docker Compose.
7. Verify service availability.

This process restores the homelab while preserving application data stored on the external drive.

---

## Deployment Checklist

- [ ] Ubuntu Server installed
- [ ] System updated
- [ ] Docker installed
- [ ] Docker Compose installed
- [ ] Git installed
- [ ] Tailscale configured
- [ ] External drive mounted
- [ ] Nextcloud filesystem mounted
- [ ] Homepage deployed
- [ ] Jellyfin deployed
- [ ] Nextcloud deployed
- [ ] Uptime Kuma deployed
- [ ] Service health verified