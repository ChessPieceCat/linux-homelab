# Nextcloud

## Purpose

Nextcloud provides private cloud storage for the Linux Homelab, allowing files to be uploaded, synchronized, and accessed securely from multiple devices.

Unlike commercial cloud storage providers, Nextcloud gives complete control over data storage, user management, and application updates while remaining entirely self-hosted.

---

## Why This Service

Nextcloud was selected to provide:

* Private file synchronization
* Centralized document storage
* Self-hosted cloud functionality
* A platform for learning Docker, databases, Linux permissions, and storage management

Because Nextcloud relies heavily on persistent storage and database integration, it became one of the most technically challenging services deployed within the homelab.

---

## Architecture

Nextcloud consists of two containers:

| Container | Purpose          |
| --------- | ---------------- |
| Nextcloud | Web application  |
| MariaDB   | Database backend |

The application container communicates with the MariaDB container through the Docker Compose network while storing uploaded files on persistent storage outside of the container.

---

## Docker Configuration

Nextcloud is deployed as an independent Docker Compose project.

The deployment includes:

* One Nextcloud container
* One MariaDB container
* Persistent database storage
* Persistent application data stored on the external drive

Separating the application and database into individual containers simplifies maintenance and follows common containerization practices.

---

## Persistent Storage

Persistent storage is one of the most important aspects of the Nextcloud deployment.

Application files uploaded by users are stored on an ext4 filesystem mounted from a disk image located on the external NTFS drive.

The MariaDB database stores metadata, user accounts, file information, and application configuration.

Because both application data and database files are stored outside of their respective containers, the containers can be recreated without affecting user data.

---

## Networking

The Nextcloud web application is exposed on port **8081**.

Communication between Nextcloud and MariaDB occurs only within the Docker Compose network.

Remote administration is performed through Tailscale, avoiding unnecessary exposure of the service to the public Internet.

---

## Security Considerations

Several measures improve the security of the deployment:

* Remote access is provided through Tailscale instead of public port forwarding.
* Nextcloud restricts access using trusted domains.
* Persistent application data is separated from container filesystems.
* The database is isolated within the Docker Compose project.

These decisions reduce the attack surface while simplifying administration.

---

## Routine Maintenance

Routine maintenance includes:

* Updating Docker images.
* Recreating containers after updates.
* Verifying database connectivity.
* Confirming storage mounts.
* Monitoring service availability through Uptime Kuma.
* Backing up application data and the MariaDB database.

---

## Lessons Learned

Deploying Nextcloud provided experience with several important infrastructure concepts.

The most significant lessons included:

* Linux filesystems and permissions directly affect application behavior.
* Containers should never store important application data internally.
* Separating databases from applications simplifies maintenance.
* Proper storage planning is critical for long-term reliability.
* Docker Compose simplifies multi-container application deployment while keeping services logically organized.

Troubleshooting filesystem permissions and designing a storage architecture compatible with Nextcloud significantly improved understanding of Linux storage management.

---

## Future Improvements

Potential improvements include:

* Automated database backups.
* Reverse proxy with HTTPS.
* Single Sign-On (SSO).
* Automated container updates.
* Additional Nextcloud applications and integrations.
* Monitoring storage utilization and database performance.

---

## Operational Commands

Check container status:

```bash
docker compose ps
```

View application logs:

```bash
docker logs nextcloud
```

View database logs:

```bash
docker logs nextcloud-db
```

Restart the service:

```bash
docker compose up -d
```

Update containers:

```bash
docker compose pull
docker compose up -d
```