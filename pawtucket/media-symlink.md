# Pawtucket Media Symlink
Pawtucket does not store media files itself. All images, thumbnails, and derivatives are generated and stored by Providence. To ensure Pawtucket displays media correctly, you must link Pawtucket’s media directory to Providence’s media directory.

This page describes how to create that symlink under WSL2.

## 1. Remove Pawtucket’s default media directory
Pawtucket ships with its own media folder, but it must be replaced with a symbolic link.

Remove it:

Code
sudo rm -rf /var/www/html/capublic/media
This ensures Pawtucket does not attempt to use its own media directory.

## 2. Create the symbolic link
Link Pawtucket’s media directory to Providence’s media directory:

Code
sudo ln -s /var/www/html/ca/media /var/www/html/capublic/media
This creates:

Code
/var/www/html/capublic/media -> /var/www/html/ca/media
## 3. Verify the symlink
Run:

Code
ls -l /var/www/html/capublic/media
Expected output:

Code
media -> /var/www/html/ca/media
If you see this, the symlink is correct.

## 4. Check permissions
Providence’s media directory must be readable by Apache:

Code
sudo chown -R www-data:www-data /var/www/html/ca/media
sudo chmod -R 775 /var/www/html/ca/media
Pawtucket does not need write access — only read access.

## 5. Test media loading
Open Pawtucket:

Code
http://localhost/capublic
Check:

Object images

Thumbnails

Derivatives

Collection images

Entity portraits

If images appear, the symlink is working.

If images do not appear:

Ensure Providence’s media directory exists

Ensure Providence has generated derivatives

Ensure permissions are correct

Ensure Pawtucket’s setup.php points to the correct base directory

Ensure the symlink path is correct

## 6. Notes for users migrating from Docker
Docker installations often used:

Code
/var/www/html/capublic/media -> /var/www/html/ca/media
The same structure works under WSL2.

However, if your Docker installation used a different path (e.g., /var/www/html/html/media), update the symlink accordingly.

## 7. Next steps
Continue with:

Performance tuning → performance/opcache.md

MySQL tuning → performance/mysql-tuning.md

PHP‑FPM + Apache mpm_event → performance/php-fpm.md
