# Storage Architecture

## Purpose

This document describes how persistent storage is organized within the Linux Homelab. It explains the physical storage devices, filesystem layout, mount points, Docker data locations, and the design decisions that influenced the overall storage architecture.

The storage design prioritizes reliability, maintainability, and separation of operating system files, application data, and media while minimizing the risk of data loss during upgrades or container recreation.

---

## Storage Overview

The homelab uses two physical storage devices:

| Device            | Purpose                                                                              |
| ----------------- | ------------------------------------------------------------------------------------ |
| Internal NVMe SSD | Ubuntu Server installation, Docker Engine, Docker Compose projects, and system files |
| External 2 TB HDD | Media library, application data, and persistent storage                              |

Separating the operating system from persistent application data allows the server to be updated or rebuilt without affecting user data stored on the external drive.

---

<p align="center">
    <img src="assets/architecture/storage-overview.svg" alt="Linux Homelab Architecture" width="900">
</p>

---

## Filesystem Layout

The storage layout is organized as follows:

* The Ubuntu Server installation resides on the internal NVMe SSD.
* Docker Engine and Docker Compose configurations are stored on the internal SSD.
* Media libraries are stored directly on the external hard drive.
* Nextcloud data is stored on an ext4 filesystem contained within a disk image located on the external drive.

This separation keeps frequently accessed system files on fast storage while reserving the larger external drive for long-term data storage.

---

## Mount Points

The homelab currently uses the following mount points:

| Mount Point      | Purpose                                                           |
| ---------------- | ----------------------------------------------------------------- |
| `/mnt/external`  | External storage containing media and persistent application data |
| `/mnt/nextcloud` | Mounted ext4 filesystem used as the Nextcloud data directory      |

The external drive is mounted during system startup, followed by the Nextcloud disk image, ensuring that required storage is available before Docker services are started.

---

## Docker Persistent Storage

Containers are treated as disposable components. Persistent data is stored outside of containers using bind mounts and Docker volumes.

Examples include:

* Jellyfin configuration and cache
* Homepage configuration
* Uptime Kuma monitoring data
* MariaDB database files
* Nextcloud application data

This approach allows containers to be recreated or updated without affecting user data or application configuration.

---

## Design Decisions

Several design decisions influenced the storage architecture:

* Separate operating system files from user data.
* Keep persistent application data outside of containers.
* Preserve the existing NTFS storage device without requiring repartitioning or reformatting.
* Provide Nextcloud with a native Linux filesystem supporting POSIX permissions.
* Organize storage in a way that supports future expansion and backup strategies.

---

## Future Improvements

Potential enhancements to the storage architecture include:

* Automated backups of databases and application data.
* Snapshot-based backup strategy.
* Off-site backup replication.
* Storage health monitoring using SMART diagnostics.
* Expansion to redundant storage using RAID or NAS technologies.

---

## Failure Scenarios

### Internal SSD Failure

The operating system can be reinstalled while preserving data stored on the external drive.

### Container Failure

Containers can be recreated from Docker Compose without affecting persistent application data.

### External Drive Failure

Application data and media would require restoration from backups.
