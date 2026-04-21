# Troubleshooting

## Overview

This document describes real-world issues encountered while building and operating the Raspberry Pi 5 self-hosted server.

The focus is on identifying root causes and documenting solutions, rather than listing commands without context.

---

## 1. Navidrome shows library but cannot play files

### Symptoms

* Music library appears correctly in the UI
* Artists, albums, and tracks are visible
* Playback fails or tracks cannot be opened

### Root Cause

The NFS mount providing the media directory was not active.

As a result:

* Navidrome scanned an empty or incorrect directory
* Metadata was stored in the database
* Actual audio files were not accessible

This created a “ghost library” — visible but unusable.

### Fix

* Verify mount point:

```bash
ls /srv/media/music
```

* If empty, check mounts:

```bash
mount | grep srv
```

* Fix `/etc/fstab` configuration
* Remount:

```bash
sudo mount -a
```

### Lesson

Applications may not fail explicitly when storage is missing.
Always verify that underlying storage is correctly mounted.

---

## 2. NFS mount not available after reboot

### Symptoms

* Media directories are empty after reboot
* Services start normally but behave incorrectly
* Manual mount fixes the issue temporarily

### Root Cause

* Incorrect or incomplete `/etc/fstab` configuration
* Mount timing issues during boot

### Fix

* Verify `/etc/fstab` entry
* Ensure correct options (e.g. `_netdev`)
* Test with:

```bash
sudo mount -a
```

* Reboot and confirm mount persists

### Lesson

Persistent storage mounts must be validated across reboots, not only manually.

---

## 3. Kavita does not detect PDF files

### Symptoms

* Files exist on disk
* Kavita library appears empty or incomplete

### Root Cause

Directory structure was not compatible with Kavita’s expected layout.

### Fix

* Organize files into proper subdirectories
* Ensure library path points to correct root folder
* Trigger library rescan

### Lesson

Applications often depend on expected directory structures, not just file presence.

---

## 4. Remote access issues (Meshnet vs Tailscale)

### Symptoms

* Remote access worked inconsistently
* Connectivity varied between devices (especially mobile)
* Media streaming was unreliable

### Root Cause

Initial setup used NordVPN Meshnet, which introduced:

* inconsistent routing behavior
* unreliable performance for media streaming
* configuration complexity

### Fix

* Replaced Meshnet with Tailscale
* Reconfigured remote access through Tailscale network

### Result

* More stable connectivity
* Better compatibility with mobile devices
* Predictable behavior with Docker services

### Lesson

VPN/remote access choice has a direct impact on usability.
Simpler and more reliable tools often outperform more complex setups.

---

## 5. Services running but not behaving correctly

### Symptoms

* Containers are running (`docker ps`)
* Web UI loads
* Functionality is broken or incomplete

### Root Cause

Mismatch between:

* service configuration
* storage availability
* network routing

### Fix

Systematic debugging approach:

1. Check container status:

```bash
docker ps
```

2. Check logs:

```bash
docker logs <container>
```

3. Verify file system paths
4. Confirm network connectivity

### Lesson

A running container does not guarantee a working service.
Always validate dependencies (storage, network, config).

---

## Debugging Approach

The following workflow proved effective:

```text
Check service → Check logs → Check storage → Check network → Fix → Verify
```

Key principles:

* avoid guessing
* isolate variables
* verify assumptions step by step

---

## Summary

Most issues were not caused by application bugs, but by:

* storage misconfiguration
* incorrect mounts
* network setup choices
* assumptions about system state

Understanding how these layers interact is essential in self-hosted environments.
