# Installation Methods
You can install CollectiveAccess on Windows using either:

WSL‑Native (recommended) — fastest, simplest, most stable

Docker Desktop — containerised, portable, slower on Windows

## WSL‑Native is faster because it runs CollectiveAccess on a real Linux filesystem (ext4) with near‑native performance, avoiding Docker’s overlay filesystem, NTFS penalties, and container virtualization overhead.

### Recommended: WSL‑Native Installation
See: wsl/install-wsl.md

### Alternative: Docker Desktop Installation
See: https://github.com/<yourname>/collectiveaccess-docker-starter-kit
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

Code
/wsl/          → WSL installation and configuration guides
/providence/   → Providence setup notes and configuration checks
/pawtucket/    → Pawtucket setup notes and media linking
/performance/  → Optional tuning for PHP, MySQL, and Apache
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
https://github.com/<yourname>/collectiveaccess-docker-starter-kit

This method is portable and familiar to many users, but slower on Windows due to virtualization and overlay filesystem overhead.

### Goals of This Repository
This project aims to provide:

A modern, fast, Windows‑friendly installation path

Clear documentation for both Providence and Pawtucket

A stable environment for development, testing, and production

A better alternative to Docker Desktop for Windows users

Optional performance tuning for advanced setups
