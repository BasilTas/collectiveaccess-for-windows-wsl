# WSL Installation Guide

This page describes how to install and prepare Windows Subsystem for Linux (WSL2) for running CollectiveAccess on Windows. WSL2 provides a fast, native‑Linux environment without the overhead of Docker Desktop or IIS.

## 1. Install WSL2
Open PowerShell (Administrator) and run:

Code
wsl --install -d Ubuntu
This will:

Enable WSL

Enable the Virtual Machine Platform

Install Ubuntu

Set WSL2 as the default version

Restart Windows when prompted.

## 2. Launch Ubuntu
Open Ubuntu from the Start Menu.

On first launch, Ubuntu will:

Complete installation

Ask you to create a Linux username

Ask you to create a Linux password

These credentials are for your Linux environment only.

## 3. Update Ubuntu
Inside Ubuntu, run:

Code
sudo apt update
sudo apt upgrade -y
This ensures all packages are current.

## 4. Basic WSL commands
Useful commands for daily use:

Code
wsl -d Ubuntu        # Start Ubuntu from PowerShell
exit                 # Leave Ubuntu
wsl --shutdown       # Fully stop all WSL instances
WSL automatically suspends when not in use. No manual shutdown is required.

## 5. Accessing WSL files from Windows
You can browse and edit WSL files using Windows tools via:

Code
\\wsl$\Ubuntu\
Your web root will be:

Code
\\wsl$\Ubuntu\var\www\html
This allows editing with:

Notepad++

VS Code

Sublime

Any Windows editor

If you need write access, temporarily change ownership:

Code
sudo chown -R $USER:$USER /var/www/html
Restore ownership after editing:

Code
sudo chown -R www-data:www-data /var/www/html
## 6. Next steps
Continue with:

Apache + PHP 8.4 installation → wsl/apache-php84.md

MySQL setup → wsl/mysql-setup.md

Providence setup → providence/setup-notes.md

Pawtucket setup → pawtucket/setup-notes.md
