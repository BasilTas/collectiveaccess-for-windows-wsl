# Providence Setup Notes
This page describes how to install and configure the Providence backend of CollectiveAccess under WSL2. The recommended method is to clone Providence directly from GitHub for a clean, up‑to‑date, Linux‑native installation.

## 1. Recommended: Install Providence directly from GitHub
Cloning from GitHub provides:

the latest stable code

no Windows filesystem overhead

no leftover Docker configuration

clean permissions

easy updates (git pull)

Clone Providence into the Apache web root:

```bash
sudo git clone https://github.com/collectiveaccess/providence.git /var/www/html/ca
```
This creates:

```bash
/var/www/html/ca
```
as your Providence installation directory.

## 2. Install Composer dependencies
Providence requires vendor libraries that are not included in the GitHub repository.

Run Composer inside the Providence directory:

```bash
cd /var/www/html/ca
sudo -u www-data composer install --no-dev
```
This ensures all PHP 8.4‑compatible dependencies are installed.

## 3. Set correct permissions
Providence must be readable by Apache and writable by the installer.

```bash
sudo chown -R www-data:www-data /var/www/html/ca
sudo chmod -R 775 /var/www/html/ca
```

If you need to edit files using Windows tools:

```bash
sudo chown -R $USER:$USER /var/www/html/ca
```

After editing:

```bash
sudo chown -R www-data:www-data /var/www/html/ca
```

## 4. Configure database settings
Open:

```bash
/var/www/html/ca/setup.php
```
Ensure the database host is correct:

```bash
'localhost'
```
If you previously used Docker, change:

```bash
'mysql'
```
to:

```bash
'localhost'
```
This is essential for WSL.

## 5. Verify URL root
Providence must use:

```bash
define("__CA_URL_ROOT__", "/ca");
```
This matches the directory:

```bash
/var/www/html/ca
```

## 6. Verify base directory
Ensure:

```bash
define("__CA_BASE_DIR__", "/var/www/html/ca");
```
This is the correct path under WSL.

## 7. Verify media directory
Providence should use:

```bash
define("__CA_MEDIA_ROOT__", "/var/www/html/ca/media");
```
Ensure the directory exists:

```bash
sudo mkdir -p /var/www/html/ca/media
sudo chown -R www-data:www-data /var/www/html/ca/media
```

## 8. Alternative: Copy Providence from Windows
If you prefer to use an existing Windows copy:

```bash
sudo cp -r /mnt/c/collectiveaccess/ca /var/www/html/
```
Then apply:

permissions

database host fix

URL root check

media directory check

This method works, but cloning from GitHub is cleaner and faster.

## 9. Run the Providence installer
Open:

```bash
http://localhost/ca
```

Enter:

Database host: localhost

Database name: ca

User: causer

Password: capass

Installation should complete significantly faster than Docker Desktop.

## 10. Post‑installation permissions
After installation, ensure Providence can write to:

```bash
/var/www/html/ca/app/tmp
/var/www/html/ca/media
```

Fix permissions if needed:

```bash
sudo chown -R www-data:www-data /var/www/html/ca
sudo chmod -R 775 /var/www/html/ca
```

## 11. Next steps
Continue with:

Pawtucket setup → pawtucket/setup-notes.md

Media symlink → pawtucket/setup-notes.md

Performance tuning → performance/opcache.md
