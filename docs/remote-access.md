# Remote Access

## Overview

This document describes how remote access was implemented for the Raspberry Pi 5 self-hosted server and compares two approaches:

* NordVPN Meshnet
* Tailscale

The goal was to access self-hosted services (Navidrome, Kavita, Nextcloud) securely from outside the local network.

---

## Requirements

The remote access solution needed to:

* provide secure connectivity without exposing services publicly
* work reliably across different devices (especially mobile)
* support media streaming (music via Navidrome)
* be simple to maintain

---

## NordVPN Meshnet (Initial Approach)

### Setup

NordVPN Meshnet was initially used to enable remote connectivity between devices.

### Observed Behavior

* basic connectivity worked
* some devices were able to reach the server
* configuration was not always predictable

### Issues

* inconsistent connectivity between devices
* unreliable behavior for media streaming
* additional complexity when debugging connection problems
* less transparent networking behavior

### Conclusion

While Meshnet worked in some cases, it introduced enough inconsistency to make it less suitable for a stable self-hosted environment.

---

## Tailscale (Final Solution)

### Setup

Tailscale was introduced as a replacement for Meshnet.

### Observed Behavior

* quick and simple setup
* stable connections across all tested devices
* reliable performance for media streaming
* predictable networking behavior

### Advantages

* works consistently on mobile devices
* minimal configuration required
* integrates well with existing services
* simplifies remote debugging

---

## Comparison

| Feature           | Meshnet        | Tailscale       |
| ----------------- | -------------- | --------------- |
| Setup complexity  | Medium         | Low             |
| Reliability       | Inconsistent   | High            |
| Mobile support    | Variable       | Excellent       |
| Streaming support | Unreliable     | Stable          |
| Debugging         | More difficult | Straightforward |

---

## Final Choice

Tailscale was selected as the primary remote access solution due to:

* better reliability
* simpler setup
* consistent behavior across devices

---

## Lessons Learned

* remote access solutions should be evaluated in real usage scenarios
* theoretical features are less important than reliability
* simplicity often leads to better long-term stability
* consistent networking behavior is critical for self-hosted services

---

## Summary

Reliable remote access is a core part of a self-hosted system.

Choosing the right tool can significantly impact:

* usability
* performance
* and ease of maintenance

In this case, Tailscale provided a more stable and predictable solution than Meshnet.
