# WSL Troubleshooting
This page lists common issues encountered when installing or running CollectiveAccess under WSL2, along with clear fixes. These cover Apache, PHP, MySQL, permissions, symlinks, and configuration problems that typically arise during Providence and Pawtucket setup.

## 1. Apache shows PHP code instead of executing it
Cause
PHP module not enabled, or Apache still using mpm_prefork without mod_php.

Fix
Enable PHP 8.4:

Code
sudo a2enmod php8.4
sudo systemctl restart apache2
If using PHP‑FPM, ensure mod_php is disabled:

Code
sudo a2dismod php8.4
And confirm the FPM handler is configured in:

Code
/etc/apache2/sites-available/000-default.conf
## 2. “Database connection failed” during Providence installer
Cause
Wrong hostname — usually leftover Docker config.

Fix
Open:

Code
/var/www/html/ca/setup.php
Change:

Code
'mysql'
to:

Code
'localhost'
Also confirm the user exists:

Code
mysql -u causer -p
## 3. Pawtucket loads but images do not appear
Cause
Media symlink missing or incorrect.

Fix
Remove Pawtucket’s media folder:

Code
sudo rm -rf /var/www/html/capublic/media
Recreate symlink:

Code
sudo ln -s /var/www/html/ca/media /var/www/html/capublic/media
Check permissions:

Code
sudo chown -R www-data:www-data /var/www/html/ca/media
## 4. Providence installer fails to write files
Cause
Permissions incorrect on CA directories.

Fix
Set correct ownership:

Code
sudo chown -R www-data:www-data /var/www/html/ca
sudo chmod -R 775 /var/www/html/ca
Ensure:

Code
/var/www/html/ca/app/tmp
/var/www/html/ca/media
are writable.

## 5. Composer fails with “permission denied”
Cause
Composer cannot write vendor files as root or wrong user.

Fix
Run Composer as Apache user:

Code
sudo -u www-data composer install --no-dev
Or temporarily give yourself ownership:

Code
sudo chown -R $USER:$USER /var/www/html/ca
composer install --no-dev
sudo chown -R www-data:www-data /var/www/html/ca
## 6. Apache returns 403 Forbidden
Cause
Directory permissions or missing AllowOverride.

Fix
Check permissions:

Code
sudo chmod -R 775 /var/www/html
Enable .htaccess support:

Code
sudo a2enmod rewrite
Edit:

Code
sudo nano /etc/apache2/apache2.conf
Find:

Code
<Directory /var/www/>
Ensure:

Code
AllowOverride All
Restart:

Code
sudo systemctl restart apache2
## 7. MySQL refuses connection even with correct credentials
Cause
MySQL not running or socket authentication issues.

Fix
Start MySQL:

Code
sudo systemctl start mysql
Check status:

Code
systemctl status mysql
If root login fails:

Code
sudo mysql
Then create a proper user:

Code
CREATE USER 'causer'@'localhost' IDENTIFIED BY 'capass';
GRANT ALL PRIVILEGES ON ca.* TO 'causer'@'localhost';
FLUSH PRIVILEGES;
## 8. Pawtucket homepage loads but CSS/JS missing
Cause
Incorrect URL root.

Fix
Open:

Code
/var/www/html/capublic/setup.php
Ensure:

Code
define("__CA_URL_ROOT__", "/capublic");
Restart Apache.

## 9. Providence loads but thumbnails are broken
Cause
Derivatives not generated or Imagick missing.

Fix
Install Imagick:

Code
sudo apt install -y php8.4-imagick
sudo systemctl restart apache2
Regenerate derivatives in Providence:

Tools → Media → Regenerate Derivatives

## 10. WSL filesystem appears slow
Cause
Working inside /mnt/c/ (NTFS).

Fix
Always work inside WSL’s native filesystem:

Code
/var/www/html/
Never install CA inside:

Code
/mnt/c/
Performance improves dramatically.

## 11. Git clone fails with “permission denied”
Cause
Cloning into a directory owned by www-data.

Fix
Give yourself ownership temporarily:

Code
sudo chown -R $USER:$USER /var/www/html
Clone:

Code
git clone https://github.com/collectiveaccess/providence.git
Restore ownership:

Code
sudo chown -R www-data:www-data /var/www/html
12. Apache shows blank page or 500 error
Cause
Missing vendor libraries (Composer not run).

Fix
Run:

Code
cd /var/www/html/ca
sudo -u www-data composer install --no-dev
Repeat for Pawtucket:

Code
cd /var/www/html/capublic
sudo -u www-data composer install --no-dev
