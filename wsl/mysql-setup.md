# MySQL Setup
This page describes how to install, configure, and prepare MySQL for use with CollectiveAccess under WSL2. You will create the database, user, and permissions required for Providence and Pawtucket.

## 1. Install MySQL Server
If you followed the Apache/PHP installation page, MySQL may already be installed.
If not, install it now:

Code
sudo apt install -y mysql-server
Start and enable the service:

Code
sudo systemctl enable mysql
sudo systemctl start mysql

## 2. Secure the MySQL installation (optional but recommended)
Run:

Code
sudo mysql_secure_installation
Recommended answers:

Validate password plugin: No

Change root password: No (WSL uses socket auth)

Remove anonymous users: Yes

Disallow remote root login: Yes

Remove test database: Yes

Reload privilege tables: Yes

## 3. Create the CollectiveAccess database
Enter the MySQL shell:

Code
sudo mysql
Create the database with the correct character set:

Code
CREATE DATABASE ca CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

## 4. Create the CollectiveAccess user
Create a dedicated user for CA:

Code
CREATE USER 'causer'@'localhost' IDENTIFIED BY 'capass';
Grant full privileges on the CA database:

Code
GRANT ALL PRIVILEGES ON ca.* TO 'causer'@'localhost';
FLUSH PRIVILEGES;
Exit MySQL:

Code
EXIT;

## 5. Test the connection
Run:

Code
mysql -u causer -p
Enter the password:

Code
capass
You should see the MySQL prompt.
Exit:

Code
EXIT;

## 6. Notes for users migrating from Docker
If you previously used Docker Desktop:

Docker used the hostname mysql

WSL uses localhost

You must update setup.php in both Providence and Pawtucket

Example fix:

Code
define("__CA_DB_HOST__", "localhost");
This is essential for CA to connect to MySQL under WSL.

## 7. Next steps
Continue with:

Providence installation → providence/setup-notes.md

Pawtucket installation → pawtucket/setup-notes.md

Media symlink setup → pawtucket/setup-notes.md
