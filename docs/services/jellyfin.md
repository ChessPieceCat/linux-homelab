# Jellyfin

## Purpose

Jellyfin provides a self-hosted media server for streaming movies, television shows, and other personal media to devices on the local network or through secure remote access.

Unlike commercial streaming platforms, Jellyfin gives complete control over the media library without subscription fees or reliance on third-party services.

---

## Why This Service

Jellyfin was selected to provide:

* Self-hosted media streaming
* Organization of personal media libraries
* Metadata retrieval and artwork management
* Experience deploying containerized multimedia applications

The service also provided practical experience working with Docker bind mounts and host networking.

---

## Architecture

Jellyfin is deployed as a single Docker container.

Unlike most services in the homelab, Jellyfin uses **host networking** instead of Docker bridge networking. This simplifies media streaming and supports local network discovery features used by media clients.

Persistent configuration, cache, and media files are stored outside of the container.

---

## Docker Configuration

The Jellyfin deployment includes:

* One Jellyfin container
* Persistent configuration directory
* Persistent cache directory
* Bind-mounted media library
* Host networking

The media library is mounted directly from the external storage device.

---

## Persistent Storage

Persistent storage consists of three locations:

| Location  | Purpose                                   |
| --------- | ----------------------------------------- |
| `/config` | Jellyfin configuration                    |
| `/cache`  | Application cache                         |
| `/media`  | Movies, television shows, and other media |

Separating configuration from media simplifies maintenance and allows containers to be recreated without affecting application settings or media files.

---

## Networking

Jellyfin operates using Docker host networking.

Unlike bridge networking, host networking allows Jellyfin to communicate directly with the host network interface. This improves compatibility with media streaming protocols and network discovery features.

Remote access is provided through Tailscale.

---

## Security Considerations

The Jellyfin deployment follows several security principles:

* Media files remain outside the container.
* Configuration is stored persistently.
* Remote access is limited through Tailscale.
* The service is not directly exposed to the public Internet.

---

## Routine Maintenance

Routine maintenance includes:

* Updating the Jellyfin Docker image.
* Refreshing media libraries after adding new content.
* Verifying media mounts.
* Monitoring service health using Uptime Kuma.
* Backing up the configuration directory.

---

## Lessons Learned

Deploying Jellyfin reinforced several important infrastructure concepts.

Key lessons include:

* Containers should access media through bind mounts rather than storing data internally.
* Host networking can simplify applications that rely on local network discovery.
* Separating configuration, cache, and media improves maintainability.
* Docker makes upgrading media server software straightforward while preserving configuration and user libraries.

---

## Future Improvements

Potential improvements include:

* Hardware-accelerated video transcoding.
* Reverse proxy with HTTPS.
* Automated media library updates.
* Integration with additional media management services.
* Automated configuration backups.

---

## Operational Commands

The following commands are commonly used when administering the Jellyfin deployment.

### Navigate to the Project Directory

```bash
cd ~/jellyfin
```

### Check Container Status

```bash
docker compose ps
```

### View Running Containers

```bash
docker ps
```

### View Logs

```bash
docker logs jellyfin
```

### Restart the Service

```bash
docker compose up -d
```

### Stop the Service

```bash
docker compose down
```

### Update Container Images

```bash
docker compose pull
docker compose up -d
```

### Verify Media Mount

```bash
ls /mnt/external/media
```

### Verify Disk Usage

```bash
df -h
```
