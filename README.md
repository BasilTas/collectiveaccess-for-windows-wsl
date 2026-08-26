## Project Purpose
This repository provides a complete, fast, and reliable method for running
CollectiveAccess on Windows using WSL‑Native. It includes installation guides,
performance tuning, troubleshooting, and configuration notes for Providence and
Pawtucket.

The goal is to make CollectiveAccess run on Windows with near‑Linux performance
without relying on Docker Desktop or slow NTFS mounts.

## Why WSL‑Native Is Faster
WSL‑Native runs CollectiveAccess directly on a real Linux filesystem (ext4) with
near‑native performance. This avoids the major bottlenecks of Docker Desktop on
Windows, including:

- overlay filesystem overhead
- NTFS bind‑mount penalties
- container virtualization layers
- slow metadata operations
- slow derivative generation

Real‑world testing shows dramatically faster installation, indexing, and media
processing compared to Docker-based setups.

### Supported Windows Versions
This installation method requires modern WSL2 support and is recommended only for:

- Windows 10 version 21H2 or later
- Windows 11
- Windows Server 2022 or later

Windows Server 2019 and earlier Windows 10 builds do not support the unified WSL
package and require legacy manual installation steps. For this reason, they are
not recommended for running CollectiveAccess under WSL2.

## Upstream Repositories
This project is an independent companion guide for installing and running
CollectiveAccess on Windows using WSL‑Native. It works directly with the official
CollectiveAccess applications:

- **Providence** (backend)
- **Pawtucket2** (frontend)

Upstream sources:
- https://github.com/collectiveaccess/providence
- https://github.com/collectiveaccess/pawtucket2

This repository does not modify or replace the official codebases; it provides a
Windows‑friendly installation and performance workflow.

## Repository Structure
The documentation is organised into clear sections:

- **wsl/** — WSL installation, configuration, troubleshooting
- **providence/** — backend setup notes and configuration
- **pawtucket/** — frontend setup notes and media symlink guide
- **performance/** — MySQL, PHP‑FPM, Opcache, and WSL performance tuning
- **docker/** — legacy Docker notes (slower, not recommended)


# Installation Methods
You can install CollectiveAccess on Windows using either:

WSL‑Native (recommended) — fastest, simplest, most stable

Docker Desktop — containerised, portable, slower on Windows

### Recommended: WSL‑Native Installation
See: wsl/install-wsl.md

### Alternative: Docker Desktop Installation
See: https://github.com/BasilTas/collectiveaccess-docker-starter-kit
Why WSL‑Native is Recommended
WSL2 provides a near‑native Linux environment on Windows, giving CollectiveAccess significantly better performance than Docker Desktop or IIS. Real‑world benchmarks show:

40% faster installation (497 s vs ~844 s under Docker)

Instant form loading (e.g., New → Entity)

Faster metadata bundle parsing

Faster search index rebuilds

Faster derivative generation

No Docker overlay filesystem overhead

No NTFS bind‑mount penalties

If you are running CollectiveAccess on Windows, WSL‑Native is the preferred method.

### Repository Structure
This repository contains:
```text
Code
/wsl/          → WSL installation and configuration guides
/providence/   → Providence setup notes and configuration checks
/pawtucket/    → Pawtucket setup notes and media linking
/performance/  → Optional tuning for PHP, MySQL, and Apache
```
Each section provides step‑by‑step instructions tailored for Windows users running CA under WSL2.

### Migrating from Docker Desktop
If you previously installed CollectiveAccess using Docker Desktop, you can migrate to WSL‑Native by:

Copying your CA directories into WSL

Updating setup.php and post-setup.php hostnames

Recreating the media symlink

Importing your MySQL database

Verifying permissions

See: wsl/migrating-from-docker.md (create this file when ready).

### About the Docker Version
The Docker installation guide remains available for users who prefer containerisation:

### Alternative: Docker Desktop Installation  
https://github.com/BasilTas/collectiveaccess-docker-starter-kit

This method is portable and familiar to many users, but slower on Windows due to virtualization and overlay filesystem overhead.

### Goals of This Repository
This project aims to provide:

A modern, fast, Windows‑friendly installation path

Clear documentation for both Providence and Pawtucket

A stable environment for development, testing, and production

A better alternative to Docker Desktop for Windows users

Optional performance tuning for advanced setups
## Technical Source
This documentation and the accompanying WSL installation workflow were developed with assistance from Microsoft Copilot.
