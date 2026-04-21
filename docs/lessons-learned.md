# Lessons Learned

## Overview

This document summarizes key lessons learned while building and maintaining the Raspberry Pi 5 self-hosted server.

The focus is on practical insights gained from real usage and troubleshooting.

---

## 1. Storage is a critical dependency

Applications depend heavily on storage being correctly mounted and available.

Even small issues in mounts can:

* break multiple services
* create misleading application states
* be difficult to detect at first glance

---

## 2. A running service is not the same as a working service

Docker containers may be running while the application itself is not functioning correctly.

Common causes:

* missing storage
* incorrect paths
* networking issues

Verification must go beyond container status.

---

## 3. Simplicity improves reliability

More complex solutions (e.g. certain networking setups) may introduce instability.

In this project:

* simpler tools provided better results
* fewer moving parts reduced troubleshooting effort

---

## 4. Debugging is a core skill

Most of the work was not installation, but troubleshooting.

Effective debugging required:

* isolating problems step by step
* verifying assumptions
* checking logs, storage, and network separately

---

## 5. Small systems still require real architecture thinking

Even on a Raspberry Pi:

* services interact with each other
* storage and networking matter
* configuration decisions have long-term impact

---

## 6. Separation of concerns helps maintainability

Separating:

* services (Docker)
* storage (NFS)
* access (Tailscale)

made the system easier to understand and fix.

---

## Summary

This project demonstrates that self-hosting is not just about running services, but about understanding how systems interact.

The most valuable skills gained were:

* troubleshooting
* system design thinking
* and managing dependencies between components
