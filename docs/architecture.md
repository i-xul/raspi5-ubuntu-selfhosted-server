# Architecture

## Overview

This document provides a high-level overview of the Raspberry Pi 5 self-hosted server architecture.

The system is built around a single node running multiple containerized services, supported by external storage and remote access.

---

## High-Level Diagram

```text
                Internet
                    │
             ┌──────▼──────┐
             │   Tailscale │
             └──────┬──────┘
                    │
          ┌─────────▼─────────┐
          │ Raspberry Pi 5    │
          │ Ubuntu Server     │
          ├───────────────────┤
          │ Docker Services   │
          │                   │
          │ - Navidrome       │
          │ - Kavita          │
          │ - Nextcloud       │
          └─────────┬─────────┘
                    │
          ┌─────────▼─────────┐
          │ NFS Storage       │
          │ (external device) │
          └───────────────────┘
```

---

## Components

### Raspberry Pi 5

* runs Ubuntu Server
* acts as the main application host
* manages all Docker containers

### Docker Services

* isolates applications
* simplifies deployment and updates
* provides consistency across services

### NFS Storage

* external storage for media files
* mounted under `/srv/media/...`
* shared between services

### Remote Access (Tailscale)

* secure access without port forwarding
* used for accessing services remotely
* enables consistent connectivity across devices

---

## Data Flow

1. User connects via Tailscale network
2. Request reaches Raspberry Pi 5
3. Docker service handles the request
4. Service reads data from NFS-mounted storage
5. Response is returned to the user

---

## Design Considerations

* keep compute and storage loosely coupled
* use lightweight hardware efficiently
* prioritize simplicity over complexity
* ensure services remain accessible remotely

---

## Summary

The architecture is designed to be:

* simple
* modular
* practical for real-world use

It demonstrates how a small device can host multiple services when combined with proper storage and networking design.
