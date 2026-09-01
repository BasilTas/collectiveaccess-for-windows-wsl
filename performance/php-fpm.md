# PHP‑FPM + Apache mpm_event
Switching Apache from the default mpm_prefork + mod_php to mpm_event + PHP‑FPM provides a noticeable performance boost for CollectiveAccess. This configuration improves concurrency, reduces memory usage, and eliminates the legacy prefork model that slows down PHP applications.

Under WSL2, PHP‑FPM performs significantly better than mod_php due to native ext4 filesystem access and reduced process overhead.

## 1. Install PHP‑FPM
Install PHP‑FPM for PHP 8.4:

```bash
sudo apt install -y php8.4-fpm
```
Start and enable the service:

```bash
sudo systemctl enable php8.4-fpm
sudo systemctl start php8.4-fpm
```

## 2. Disable mod_php
Apache’s default PHP handler must be disabled:

```bash
sudo a2dismod php8.4
```
This removes the prefork dependency.

## 3. Enable Apache mpm_event
Switch Apache to the modern event‑driven MPM:

```bash
sudo a2dismod mpm_prefork
sudo a2enmod mpm_event
```

## 4. Enable Apache proxy modules
Apache needs proxy modules to pass requests to PHP‑FPM:

```bash
sudo a2enmod proxy
sudo a2enmod proxy_fcgi
sudo a2enmod setenvif
sudo a2enmod rewrite
```

## 5. Configure Apache to use PHP‑FPM
Edit the default site configuration:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```
Add the following inside the <VirtualHost *:80> block:

```bash
<FilesMatch "\.php$">
    SetHandler "proxy:unix:/run/php/php8.4-fpm.sock|fcgi://localhost/"
</FilesMatch>
```
Save and exit.

## 6. Restart Apache
Apply all changes:

```bash
sudo systemctl restart apache2
```
Verify PHP‑FPM is active:

```bash
systemctl status php8.4-fpm
```

## 7. Expected performance improvements
With PHP‑FPM + mpm_event enabled, you should see:

Faster page rendering in Providence

Faster Pawtucket front‑end response

Better concurrency under load

Lower memory usage

Faster metadata bundle loading

Faster derivative generation

Smoother navigation across tabs

Reduced Apache worker overhead

WSL2 benefits strongly from this configuration because it avoids the heavy prefork model and uses a modern event‑driven architecture.

## 8. Optional: Tune PHP‑FPM pool settings
Edit:

```bash
sudo nano /etc/php/8.4/fpm/pool.d/www.conf
```
Recommended values:

```bash
pm = dynamic
pm.max_children = 20
pm.start_servers = 4
pm.min_spare_servers = 4
pm.max_spare_servers = 10
```
Restart:

```bash
sudo systemctl restart php8.4-fpm
```

## 9. Next steps
Continue with:

Opcache tuning → performance/opcache.md

MySQL tuning → performance/mysql-tuning.md

General WSL performance tips (optional future page)
