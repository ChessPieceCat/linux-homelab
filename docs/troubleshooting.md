# Troubleshooting Guide

## Purpose

This document records significant issues encountered while building and maintaining the Linux Homelab. It serves as both a troubleshooting reference and a record of the reasoning behind various architectural decisions.

The goal is to reduce repeated troubleshooting efforts and document solutions that may be useful during future maintenance or server rebuilds.

---

## Troubleshooting Process

When diagnosing problems, the following general process is used:

1. Identify the symptoms.
2. Verify service status.
3. Review Docker logs.
4. Verify filesystem mounts and permissions.
5. Confirm network connectivity.
6. Test configuration changes incrementally.
7. Document the final solution.

Following a consistent troubleshooting process reduces the likelihood of overlooking simple configuration issues.

---

## Issue: Nextcloud Could Not Use NTFS Storage

### Symptoms

- Nextcloud could not correctly access files.
- Permission errors occurred when attempting to use the mounted NTFS drive.

### Cause

Nextcloud expects a Linux filesystem supporting POSIX ownership and permissions. The NTFS filesystem does not fully support these semantics under Linux.

### Resolution

An ext4 filesystem was created inside a disk image stored on the external drive. The image is mounted during system startup and used as the Nextcloud data directory.

### Prevention

Future Linux applications requiring POSIX permissions should be evaluated before selecting a filesystem.

---

Symptoms

Configuration disappeared after recreating containers.

Cause

Data stored inside containers is not persistent.

Resolution

Move configuration into bind mounts and Docker volumes.

---

## Issue: Homepage Could Not Detect Docker Containers

### Symptoms

* Homepage did not display container information.
* Docker widgets appeared empty or failed to update.

### Cause

Homepage requires read-only access to the Docker socket in order to query the Docker daemon. Without mounting the Docker socket into the container, Homepage cannot retrieve container status information.

### Resolution

The Docker socket was mounted into the Homepage container as a read-only volume:

```yaml
volumes:
  - /var/run/docker.sock:/var/run/docker.sock:ro
```

This allowed Homepage to retrieve container information while minimizing security risks by providing read-only access.

### Prevention

When deploying Homepage with Docker integration, verify that the Docker socket is mounted correctly and that the Homepage container has permission to access it.

---

## Issue: Jellyfin Could Not Access Media Files

### Symptoms

* Media libraries appeared empty.
* Jellyfin was unable to locate movies or television shows stored on the external drive.

### Cause

The media directory on the host system was not mounted correctly into the Jellyfin container. Without a bind mount, the container cannot access files stored outside its own filesystem.

### Resolution

The external media directory was mounted into the container using a bind mount:

```yaml
volumes:
  - /mnt/external/media:/media
```

The Jellyfin library was then configured to reference the mounted directory.

### Prevention

Verify that all required media directories are mounted correctly before configuring libraries within Jellyfin.

---

## Issue: Nextcloud Trusted Domains Prevented Access

### Symptoms

* Browser displayed a "Trusted domain error."
* Nextcloud refused connections despite the service running correctly.

### Cause

Nextcloud only accepts requests from domains and IP addresses listed in its `trusted_domains` configuration. Accessing the server from an unconfigured address causes Nextcloud to reject the request.

### Resolution

The server's IP address and any required hostnames were added to the `trusted_domains` configuration using the Nextcloud OCC command.

After updating the configuration, the web interface became accessible from the intended devices.

### Prevention

Whenever a new hostname, IP address, reverse proxy, or domain is introduced, update the `trusted_domains` configuration before attempting to access Nextcloud.

---

## Issue: Nextcloud Storage Required Correct Mount Order

### Symptoms

* Nextcloud failed to access stored data.
* Data directories appeared empty.
* Files were written to an incorrect location when the external storage was unavailable.

### Cause

The Nextcloud data directory depends on the external storage and mounted ext4 filesystem being available before the Nextcloud container starts. If the filesystem is not mounted, Docker may create an empty directory on the host, causing the container to use the wrong location.

### Resolution

The external drive and Nextcloud filesystem image were configured to mount during system startup before Docker services were started.

This ensured that the expected data directory was always available when the Nextcloud container initialized.

### Prevention

Verify storage mount points before starting Docker services. Any automated startup procedure should ensure that storage is mounted prior to launching containers.

---

## Useful Commands

The following commands are commonly used when troubleshooting the homelab:

### View Running Containers

```bash
docker ps
```

Displays all running containers and their current status.

---

### View Container Logs

```bash
docker logs <container_name>
```

Displays application logs for a specific container.

---

### Restart a Service

```bash
docker compose up -d
```

Recreates or starts containers using the current Docker Compose configuration.

---

### Pull Updated Images

```bash
docker compose pull
```

Downloads the latest container images without restarting services.

---

### Inspect Container Configuration

```bash
docker inspect <container_name>
```

Displays detailed information about container networking, volumes, environment variables, and runtime configuration.

---

### List Docker Volumes

```bash
docker volume ls
```

Displays all Docker-managed persistent volumes.

---

### Check Mounted Filesystems

```bash
mount
```

Verifies that required filesystems are mounted correctly.

---

### Check Disk Usage

```bash
df -h
```

Displays available disk space and mounted storage devices.

---

### Check Service Status

```bash
docker compose ps
```

Shows the status of services within the current Docker Compose project.
