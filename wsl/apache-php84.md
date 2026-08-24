# Apache + PHP 8.4 Installation
This page describes how to install Apache, PHP 8.4, and the required PHP extensions for running CollectiveAccess under WSL2. Ubuntu’s default PHP version is not sufficient, so we enable the official PHP PPA and install PHP 8.4 explicitly.

## 1. Enable the PHP PPA
Ubuntu 24.04 ships PHP 8.3, but CollectiveAccess requires PHP ≥ 8.4.

Add the PPA:

Code
sudo add-apt-repository ppa:ondrej/php
sudo apt update
This repository provides current PHP versions and extensions.

## 2. Install Apache and PHP 8.4
Install Apache, PHP 8.4, and all required extensions:

Code
sudo apt install -y apache2 mysql-server \
    php8.4 php8.4-cli php8.4-common php8.4-mysql php8.4-gd \
    php8.4-xml php8.4-mbstring php8.4-curl php8.4-zip \
    php8.4-intl php8.4-imagick
These extensions cover all CA requirements, including media processing and XML profile parsing.

## 3. Enable PHP 8.4 in Apache
Disable any older PHP module (e.g., PHP 8.3):

Code
sudo a2dismod php8.3
Enable PHP 8.4:

Code
sudo a2enmod php8.4
sudo systemctl restart apache2
## 4. Verify PHP installation
Check the installed version:

Code
php -v
Expected output includes:

Code
PHP 8.4.x (cli)
## 5. Test Apache + PHP
Create a simple PHP info file:

Code
echo "<?php phpinfo();" | sudo tee /var/www/html/info.php
Open in your browser:

Code
http://localhost/info.php
You should see the PHP information page, confirming Apache and PHP are working correctly.

Delete the file afterwards:

Code
sudo rm /var/www/html/info.php
## 6. Next steps
Continue with:

MySQL setup → wsl/mysql-setup.md

Providence installation → providence/setup-notes.md

Pawtucket installation → pawtucket/setup-notes.md
