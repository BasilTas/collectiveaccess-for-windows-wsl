# Pawtucket Setup Notes
This page describes how to install and configure the Pawtucket2 front‑end of CollectiveAccess under WSL2. The recommended method is to clone Pawtucket directly from GitHub for a clean, up‑to‑date installation.

## 1. Recommended: Install Pawtucket directly from GitHub
Cloning from GitHub provides:

the latest stable code

clean Linux‑native permissions

no leftover Docker configuration

easy updates (git pull)

Clone Pawtucket into the Apache web root:

Code
sudo git clone https://github.com/collectiveaccess/pawtucket2.git /var/www/html/capublic
This creates:

Code
/var/www/html/capublic
as your Pawtucket installation directory.

## 2. Install Composer dependencies
Pawtucket also requires vendor libraries that are not included in the GitHub repository.

Run Composer inside the Pawtucket directory:

Code
cd /var/www/html/capublic
sudo -u www-data composer install --no-dev
This ensures all PHP 8.4‑compatible dependencies are installed.

## 3. Set correct permissions
Pawtucket must be readable by Apache and able to access the shared media directory.

Code
sudo chown -R www-data:www-data /var/www/html/capublic
sudo chmod -R 775 /var/www/html/capublic
If you need to edit files using Windows tools:

Code
sudo chown -R $USER:$USER /var/www/html/capublic
Restore ownership afterwards:

Code
sudo chown -R www-data:www-data /var/www/html/capublic
## 4. Configure database settings
Open:

Code
/var/www/html/capublic/setup.php
Ensure the database host is correct:

Code
'localhost'
If you previously used Docker, change:

Code
'mysql'
to:

Code
'localhost'
This is essential for WSL.

## 5. Verify URL root
Pawtucket should use:

Code
define("__CA_URL_ROOT__", "/capublic");
This matches the directory:

Code
/var/www/html/capublic
## 6. Link Pawtucket to Providence’s media directory
Pawtucket does not store media itself.
It must reference Providence’s media directory.

Remove the default Pawtucket media folder:

Code
sudo rm -rf /var/www/html/capublic/media
Create a symlink pointing to Providence’s media:

Code
sudo ln -s /var/www/html/ca/media /var/www/html/capublic/media
Verify:

Code
ls -l /var/www/html/capublic/media
You should see:

Code
media -> /var/www/html/ca/media
This ensures Pawtucket displays images, thumbnails, and derivatives correctly.

## 7. Alternative: Copy Pawtucket from Windows
If you prefer to use an existing Windows copy:

Code
sudo cp -r /mnt/c/collectiveaccess/capublic /var/www/html/
Then apply:

permissions

database host fix

URL root check

media symlink

Cloning from GitHub is cleaner and avoids Windows filesystem overhead.

## 8. Test Pawtucket
Open:

Code
http://localhost/capublic
You should see the Pawtucket homepage.

If media does not appear:

check the symlink

check Providence’s media permissions

check Apache’s access rights

## 9. Next steps
Continue with:

Performance tuning → performance/opcache.md

MySQL tuning → performance/mysql-tuning.md

PHP‑FPM + Apache mpm_event → performance/php-fpm.md
