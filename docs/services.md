# Services

## Overview

This document provides a short overview of the services running on the Raspberry Pi 5 self-hosted server.

Each service has a specific purpose within the environment, and together they form a practical personal infrastructure stack.

---

## Navidrome

**Purpose:** self-hosted music streaming

Navidrome is used to serve a personal music library over the network.

### Role in the system

* streams music to browser and mobile clients
* reads media from mounted storage
* provides a lightweight Spotify-like listening experience

### Notes

* depends on correct media mount configuration
* was used together with remote access for listening outside the local network

---

## Kavita

**Purpose:** self-hosted ebook and PDF library

Kavita is used to organize and read books and documents through a web interface.

### Role in the system

* provides a personal digital library
* supports PDFs and other reading formats
* allows browser-based reading from multiple devices

### Notes

* requires correct library path setup
* works best with a clear folder structure for books

---

## Nextcloud

**Purpose:** personal cloud storage and file access

Nextcloud provides file storage, synchronization, and remote access to personal files.

### Role in the system

* centralizes file storage
* provides access across devices
* acts as part of the broader self-hosted ecosystem

### Notes

* heavier than the other services
* benefits from stable storage and proper system planning

---

## Docker

**Purpose:** service isolation and deployment management

Docker is used to run the server applications in separate containers.

### Role in the system

* isolates services from each other
* makes deployment more repeatable
* simplifies service startup and restart behavior

### Notes

* storage paths and mounts must be correct
* container health does not guarantee application health

---

## NFS Storage

**Purpose:** external media access

NFS is used to mount storage from another device into the Raspberry Pi server.

### Role in the system

* provides media files for services such as Navidrome and Kavita
* keeps storage separate from compute
* allows reuse of existing file storage

### Notes

* mount failures directly affect application behavior
* troubleshooting often starts with verifying mounted paths

---

## Tailscale

**Purpose:** secure remote access

Tailscale is used to access services remotely without exposing them directly to the public internet.

### Role in the system

* provides secure remote connectivity
* simplifies access from external devices
* improves usability for mobile clients

### Notes

* chosen for reliability and simplicity
* better fit for this setup than more complicated alternatives

---

## Summary

These services work together to provide:

* music streaming
* digital reading
* personal cloud storage
* remote access
* containerized service management

The result is a practical self-hosted environment built around real usage rather than isolated demos.
