# Uptime Kuma

## Purpose

Uptime Kuma provides continuous monitoring of services hosted within the Linux Homelab. It performs regular health checks and provides a centralized interface for monitoring service availability.

By continuously monitoring hosted applications, Uptime Kuma helps identify service interruptions quickly and verifies that services remain operational following updates or configuration changes.

---

## Why This Service

Uptime Kuma was selected to provide:

* Continuous service monitoring
* Availability reporting
* Centralized health checks
* Early detection of service failures
* Practical experience deploying monitoring infrastructure

Monitoring is an important component of infrastructure management, ensuring that problems can be identified before users are affected.

---

## Architecture

Uptime Kuma is deployed as a single Docker container using Docker Compose.

Application configuration, monitor definitions, and historical monitoring data are stored outside the container using a persistent bind mount.

---

## Docker Configuration

The deployment consists of:

* One Uptime Kuma container
* Persistent monitoring database
* Persistent configuration directory
* Web interface exposed on port **3001**

Persistent storage ensures monitoring history and configuration survive container updates.

---

## Persistent Storage

Persistent monitoring data is stored in:

| Location | Purpose                                                                                  |
| -------- | ---------------------------------------------------------------------------------------- |
| `./data` | Configuration, monitor definitions, historical monitoring data, and application database |

This directory is mounted into the container to preserve monitoring data between container recreations.

---

## Networking

Uptime Kuma uses Docker bridge networking and is accessible on port **3001**.

The service monitors applications running both locally and through Docker, allowing centralized health monitoring of the homelab.

Remote administration is provided through Tailscale.

---

## Security Considerations

The monitoring service follows several security principles:

* Administrative access is restricted to trusted devices.
* Remote access is provided through Tailscale.
* Persistent monitoring data is stored outside the container.
* Services are monitored without exposing additional public-facing endpoints.

---

## Routine Maintenance

Routine maintenance includes:

* Updating the Docker image.
* Verifying monitor status after infrastructure changes.
* Reviewing failed health checks.
* Confirming notification settings.
* Backing up the monitoring database.

---

## Lessons Learned

Deploying Uptime Kuma demonstrated the importance of continuous monitoring within self-hosted infrastructure.

Key lessons include:

* Monitoring should be treated as a core infrastructure component rather than an optional addition.
* Service health should be verified after updates or configuration changes.
* Persistent monitoring data allows historical analysis of service availability.
* Centralized monitoring simplifies troubleshooting by providing immediate visibility into service status.

---

## Future Improvements

Potential improvements include:

* Email or mobile notifications for service outages.
* Monitoring of additional infrastructure components.
* SSL certificate expiration monitoring.
* Resource utilization monitoring.
* Integration with centralized logging.

---

## Operational Commands

The following commands are commonly used when administering the Uptime Kuma deployment.

### Navigate to the Project Directory

```bash
cd ~/uptime-kuma
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
docker logs uptime-kuma
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

### Verify Persistent Data

```bash
ls ~/uptime-kuma/data
```

### Verify Monitor Status

Open the Uptime Kuma dashboard and confirm that all configured monitors report a healthy status after any deployment or infrastructure change.
