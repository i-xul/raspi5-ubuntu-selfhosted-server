# Storage and Mounts

## Overview

This document describes how storage is handled in the Raspberry Pi 5 self-hosted server, with a focus on NFS mounts and their impact on application behavior.

The system uses external storage for media files, which are mounted into the server and shared between multiple services.

---

## Storage Architecture

Media is not stored locally on the Raspberry Pi.

Instead:

* files are hosted on a separate device
* accessed over the network via NFS
* mounted into the server filesystem

Typical structure:

```text id="8d2g6n"
/srv/media/
├── music/
├── books/
└── other/
```

These paths are used directly by Docker services such as Navidrome and Kavita.

---

## Why External Storage

Reasons for using external storage:

* separates compute and storage
* allows reuse across multiple devices
* simplifies scaling storage independently
* avoids filling local disk on Raspberry Pi

---

## NFS Mount Configuration

Mounts are defined in `/etc/fstab`.

Example (sanitized):

```text id="kw3c8x"
192.168.x.x:/export/media  /srv/media  nfs  defaults,_netdev  0  0
```

Key option:

* `_netdev` ensures the mount waits for network availability during boot

---

## Common Issues

### 1. Mount not active

#### Symptoms

* directories appear empty
* applications behave incorrectly
* no obvious errors in UI

#### Cause

* mount not applied
* network not ready during boot
* incorrect fstab entry

#### Fix

```bash id="bbt8w7"
sudo mount -a
```

Then verify:

```bash id="s9u7kj"
ls /srv/media
```

---

### 2. "Ghost" media libraries

#### Symptoms

* media appears in application UI
* playback or file access fails

#### Cause

Applications scanned metadata when the mount was missing.

As a result:

* database contains entries
* actual files are not accessible

#### Fix

* ensure mount is active
* restart application
* rescan library if needed

---

### 3. Mount works manually but not after reboot

#### Symptoms

* system works after manual mount
* fails again after reboot

#### Cause

* incorrect or incomplete `/etc/fstab`
* missing network dependency

#### Fix

* verify fstab entry
* include `_netdev`
* test with reboot

---

## Impact on Applications

Storage issues directly affected:

### Navidrome

* detected music library structure
* could not play files when mount was missing

### Kavita

* failed to detect content when directory structure was incorrect
* required proper path configuration

---

## Debugging Workflow

Effective steps:

```text id="zzk8c1"
Check mount → Check directory → Check application → Restart service
```

Commands used:

```bash id="8b1cwh"
mount | grep srv
ls /srv/media
docker logs <container>
```

---

## Lessons Learned

* storage layer is critical in self-hosted systems
* missing mounts do not always produce explicit errors
* applications may appear functional while being broken
* always validate storage before debugging application logic

---

## Summary

Reliable storage configuration is essential for multi-service environments.

Even simple mistakes in mounts can:

* break multiple services
* create misleading application states
* complicate troubleshooting

Understanding how storage integrates with applications is key to maintaining a stable system.
