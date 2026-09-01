# PHP Opcache Configuration
Opcache dramatically improves CollectiveAccess performance by caching compiled PHP code in memory. This reduces filesystem access, speeds up metadata bundle loading, and improves overall responsiveness. Under WSL2, Opcache provides a noticeable improvement over Docker Desktop and IIS.

This page describes how to enable and tune Opcache for Providence and Pawtucket.

## 1. Enable Opcache
Opcache is included with PHP 8.4 but may not be enabled by default.

Edit the PHP configuration file:

```bash
sudo nano /etc/php/8.4/apache2/php.ini
```
Ensure the following line is present and uncommented:

```bash
opcache.enable=1
```
Restart Apache:

```bash
sudo systemctl restart apache2
```

## 2. Recommended Opcache settings for CollectiveAccess
Add or update the following values in php.ini:

```bash
opcache.enable=1
opcache.memory_consumption=256
opcache.interned_strings_buffer=16
opcache.max_accelerated_files=20000
opcache.revalidate_freq=2
opcache.validate_timestamps=1
opcache.fast_shutdown=1
opcache.enable_cli=0
```
Why these values?
```bash
memory_consumption=256
```  
CA has a large codebase; 256MB ensures enough room for compiled scripts.
```bash
max_accelerated_files=20000
```  
Providence + Pawtucket + vendor libraries easily exceed 10k PHP files.
```bash
revalidate_freq=2
```  
Checks for file changes every 2 seconds — safe for development.
```bash
interned_strings_buffer=16
```  
Helps with CA’s metadata-heavy string usage.

## 3. Optional: Enable JIT (PHP 8.4)
PHP 8.4 includes an improved JIT compiler. CA does not rely heavily on CPU-bound PHP loops, but enabling JIT can still improve certain operations.

In php.ini:

```bash
opcache.jit=1255
opcache.jit_buffer_size=64M
```
Restart Apache:

```bash
sudo systemctl restart apache2
```

## 4. Verify Opcache is active
Create a temporary PHP info file:

```bash
echo "<?php phpinfo();" | sudo tee /var/www/html/info.php
```
Open:

```bash
http://localhost/info.php
```
Look for the Zend OPcache section.

Delete the file afterwards:

```bash
sudo rm /var/www/html/info.php
```

## 5. Expected performance improvements
With Opcache enabled, you should see:

Faster Providence page loads

Faster Pawtucket rendering

Instant bundle loading (New → Entity, New → Object, etc.)

Faster metadata profile parsing

Reduced CPU usage

Reduced filesystem access

Smoother navigation across tabs and bundles

Under WSL2, these improvements are more noticeable than under Docker Desktop due to the native ext4 filesystem.

## 6. Next steps
Continue with:

MySQL tuning → performance/mysql-tuning.md

PHP‑FPM + Apache mpm_event → performance/php-fpm.md
