# MySQL Performance Tuning
CollectiveAccess relies heavily on MySQL for metadata storage, search indexing, and relationship resolution. Optimising MySQL under WSL2 significantly improves responsiveness, especially for large catalogues or complex metadata profiles.

This page provides recommended MySQL tuning settings for Providence and Pawtucket.

1. Edit MySQL configuration
Open the MySQL configuration file:

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
You will add or adjust several key parameters.

## 2. Recommended MySQL settings for CollectiveAccess
Add or update the following values inside mysqld.cnf:

```bash
innodb_buffer_pool_size = 1G
innodb_buffer_pool_instances = 1
innodb_log_file_size = 256M
innodb_flush_log_at_trx_commit = 2
innodb_flush_method = O_DIRECT
innodb_file_per_table = 1

max_connections = 200
query_cache_type = 0
query_cache_size = 0

tmp_table_size = 64M
max_heap_table_size = 64M
```
Why these values?
```bash
innodb_buffer_pool_size = 1G
```  
The most important setting. CA benefits enormously from a large buffer pool.
If you have >16GB RAM, you can increase this to 2G or 3G.
```bash
innodb_log_file_size = 256M
```  
Helps with large transactions and bulk imports.
```bash
innodb_flush_log_at_trx_commit = 2
```  
Safe for WSL2 and improves write performance.
```bash
query_cache_type = 0
``` 
MySQL’s query cache is deprecated and slows CA down.
```bash
tmp_table_size / max_heap_table_size = 64M
```   
Helps with complex searches and relationship queries.

## 3. Restart MySQL
Apply the changes:

```bash
sudo systemctl restart mysql
```

## 4. Verify buffer pool size
Run:

```bash
mysql -u causer -p -e "SHOW VARIABLES LIKE 'innodb_buffer_pool_size';"
```
Expected output:

```bash
innodb_buffer_pool_size | 1073741824
```
(1GB)

## 5. Expected performance improvements
With MySQL tuned, you should see:

Faster search results

Faster relationship resolution

Faster metadata bundle loading

Faster indexing

Reduced disk I/O

Smoother Providence navigation

Better performance under load

WSL2 benefits strongly from these settings because MySQL runs on a native ext4 filesystem rather than Docker’s overlay filesystem.

## 6. Optional: Increase buffer pool size
If your system has plenty of RAM:

16GB RAM → set buffer pool to 2G

32GB RAM → set buffer pool to 4G

Example:

```bash
innodb_buffer_pool_size = 2G
```
This is safe and beneficial for large CA installations.

## 7. Next steps
Continue with:

PHP‑FPM + Apache mpm_event → performance/php-fpm.md

Opcache tuning → performance/opcache.md

General WSL performance tips (optional future page)
