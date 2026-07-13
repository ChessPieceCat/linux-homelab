# Networking

## Purpose

This document describes the networking architecture of the Linux Homelab, including Docker networking, service exposure, remote access, and the security considerations that influenced the overall design.

The networking configuration is designed to provide secure access to self-hosted services while minimizing unnecessary exposure to the public Internet.

---

## Network Overview

The homelab operates primarily on the local network, with secure remote administration provided through Tailscale.

Most services are exposed only to trusted devices on the local network or through the private Tailscale mesh VPN.

---

<p align="center">
    <img src="assets/architecture/network-overview.drawio.svg" alt="Linux Homelab Architecture" width="900">
</p>

---

## Docker Networking

Most services use Docker bridge networking provided automatically by Docker Compose.

Separate Compose projects create independent Docker networks, isolating services unless communication between them is explicitly required.

Jellyfin is configured to use the host network.

This allows Jellyfin to access network discovery protocols and simplifies media streaming on the local network.

---

## Service Ports

| Service     |         Port |
| ----------- | -----------: |
| Homepage    |         3000 |
| Uptime Kuma |         3001 |
| Nextcloud   |         8081 |
| Jellyfin    | Host Network |

These ports are currently intended for trusted local network access.

---

## Remote Access

Remote administration is provided using Tailscale.

Instead of exposing services through router port forwarding, devices authenticate to a private mesh VPN before access is granted.

This approach reduces the server's public attack surface while allowing secure access from authorized devices.

---

## Security Considerations

The networking design follows several principles:

* Avoid exposing unnecessary services to the public Internet.
* Minimize open ports.
* Isolate applications through Docker networking.
* Use encrypted remote connections.
* Prefer authenticated access over public accessibility.

---

## Future Improvements

Potential networking enhancements include:

* Reverse proxy using Caddy or Nginx Proxy Manager.
* Automatic HTTPS certificates.
* Internal DNS names for hosted services.
* Single Sign-On (SSO).
* Network monitoring and traffic visualization.
