# WSL Troubleshooting
This page lists common issues encountered when installing or running CollectiveAccess under WSL2, along with clear fixes. These cover Apache, PHP, MySQL, permissions, symlinks, and configuration problems that typically arise during Providence and Pawtucket setup.

## 1. Apache shows PHP code instead of executing it
Cause
PHP module not enabled, or Apache still using mpm_prefork without mod_php.

Fix
Enable PHP 8.4:

```bash
sudo a2enmod php8.4
sudo systemctl restart apache2
```
If using PHP‑FPM, ensure mod_php is disabled:

```bash
sudo a2dismod php8.4
```
And confirm the FPM handler is configured in:

```bash
/etc/apache2/sites-available/000-default.conf
```

## 2. “Database connection failed” during Providence installer
Cause
Wrong hostname — usually leftover Docker config.

Fix
Open:

```bash
/var/www/html/ca/setup.php
```
Change:

```bash
'mysql'
```
to:

```bash
'localhost'
```
Also confirm the user exists:

```bash
mysql -u causer -p
```

## 3. Pawtucket loads but images do not appear
Cause
Media symlink missing or incorrect.

Fix
Remove Pawtucket’s media folder:

```bash
sudo rm -rf /var/www/html/capublic/media
```
Recreate symlink:

```bash
sudo ln -s /var/www/html/ca/media /var/www/html/capublic/media
```
Check permissions:

```bash
sudo chown -R www-data:www-data /var/www/html/ca/media
```

## 4. Providence installer fails to write files
Cause
Permissions incorrect on CA directories.

Fix
Set correct ownership:

```bash
sudo chown -R www-data:www-data /var/www/html/ca
sudo chmod -R 775 /var/www/html/ca
```
Ensure:

```bash
/var/www/html/ca/app/tmp
/var/www/html/ca/media
```
are writable.

## 5. Composer fails with “permission denied”
Cause
Composer cannot write vendor files as root or wrong user.

Fix
Run Composer as Apache user:

```bash
sudo -u www-data composer install --no-dev
```
Or temporarily give yourself ownership:

```bash
sudo chown -R $USER:$USER /var/www/html/ca
composer install --no-dev
sudo chown -R www-data:www-data /var/www/html/ca
```

## 6. Apache returns 403 Forbidden
Cause
Directory permissions or missing AllowOverride.

Fix
Check permissions:

```bash
sudo chmod -R 775 /var/www/html
```
Enable .htaccess support:

```bash
sudo a2enmod rewrite
```
Edit:

```bash
sudo nano /etc/apache2/apache2.conf
```
Find:

```bash
<Directory /var/www/>
```
Ensure:

```bash
AllowOverride All
```
Restart:

```bash
sudo systemctl restart apache2
```

## 7. MySQL refuses connection even with correct credentials
Cause
MySQL not running or socket authentication issues.

Fix
Start MySQL:

```bash
sudo systemctl start mysql
```
Check status:

```bash
systemctl status mysql
```
If root login fails:

```bash
sudo mysql
```
Then create a proper user:

```bash
CREATE USER 'causer'@'localhost' IDENTIFIED BY 'capass';
GRANT ALL PRIVILEGES ON ca.* TO 'causer'@'localhost';
FLUSH PRIVILEGES;
```

## 8. Pawtucket homepage loads but CSS/JS missing
Cause
Incorrect URL root.

Fix
Open:

```bash
/var/www/html/capublic/setup.php
```
Ensure:

```bash
define("__CA_URL_ROOT__", "/capublic");
```
Restart Apache.

## 9. Providence loads but thumbnails are broken
Cause
Derivatives not generated or Imagick missing.

Fix
Install Imagick:

```bash
sudo apt install -y php8.4-imagick
sudo systemctl restart apache2
```
Regenerate derivatives in Providence:

Tools → Media → Regenerate Derivatives

## 10. WSL filesystem appears slow
Cause
Working inside /mnt/c/ (NTFS).

Fix
Always work inside WSL’s native filesystem:

```bash
/var/www/html/
```
Never install CA inside:

```bash
/mnt/c/
```
Performance improves dramatically.

## 11. Git clone fails with “permission denied”
Cause
Cloning into a directory owned by www-data.

Fix
Give yourself ownership temporarily:

```bash
sudo chown -R $USER:$USER /var/www/html
```
Clone:

```bash
git clone https://github.com/collectiveaccess/providence.git
```
Restore ownership:

```bash
sudo chown -R www-data:www-data /var/www/html
```

12. Apache shows blank page or 500 error
Cause
Missing vendor libraries (Composer not run).

Fix
Run:

```bash
cd /var/www/html/ca
sudo -u www-data composer install --no-dev
```
Repeat for Pawtucket:

```bash
cd /var/www/html/capublic
sudo -u www-data composer install --no-dev
```
