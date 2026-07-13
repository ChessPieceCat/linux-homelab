# Homepage

## Purpose

Homepage provides a centralized dashboard for accessing and monitoring all services hosted within the Linux Homelab.

Rather than remembering individual IP addresses and ports, Homepage presents a single interface for launching applications, viewing service health, and monitoring Docker containers.

---

## Why This Service

As additional services were added to the homelab, managing multiple URLs and ports became increasingly inconvenient.

Homepage provides:

* A centralized dashboard
* Docker container status
* Service organization
* Quick access to applications

---

## Docker Configuration

Homepage is deployed as an independent Docker Compose project.

The container exposes port **3000** and mounts two persistent volumes:

| Volume                 | Purpose                     |
| ---------------------- | --------------------------- |
| `./config`             | Homepage configuration      |
| `/var/run/docker.sock` | Read-only Docker API access |

The Docker socket is mounted read-only to allow Homepage to retrieve container information without granting unnecessary write access.

---

## Persistent Storage

Homepage stores configuration files within the project directory.

This allows the dashboard configuration to persist across container updates and recreation.

---

## Networking

Homepage is deployed using Docker bridge networking and is available on port **3000**.

The service is accessible from trusted devices on the local network and remotely through Tailscale.

---

## Routine Maintenance

Routine maintenance consists of:

* Updating the Docker image.
* Restarting the container after configuration changes.
* Verifying Docker integration after updates.
* Confirming service availability through Uptime Kuma.

---

## Lessons Learned

Integrating Homepage with Docker requires mounting the Docker socket into the container.

Providing read-only access allows Homepage to display container status while limiting unnecessary privileges.

Homepage also demonstrated the importance of organizing infrastructure visually. As additional services were deployed, the dashboard became the primary interface for managing the homelab.

---

## Future Improvements

Potential future enhancements include:

* Additional service widgets.
* Resource utilization monitoring.
* Integration with reverse proxy URLs.
* Improved dashboard organization.
* Custom icons and categories.

---

## Operational Commands

The following commands are commonly used when administering the Homepage deployment.

### Navigate to the Project Directory

```bash
cd ~/homepage
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
docker logs homepage
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

### Verify Docker Socket Mount

```bash
docker inspect homepage
```

Confirm that the following bind mount exists:

```text
/var/run/docker.sock:/var/run/docker.sock:ro
```

### Verify Configuration Files

```bash
ls ~/homepage/config
```