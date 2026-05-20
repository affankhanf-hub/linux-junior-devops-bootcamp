# Day 13: LEMP Stack (Linux, Nginx, MySQL, PHP-FPM)

## Theory Summary

### 1. WHAT IS LEMP?

LEMP = Linux + Nginx + MySQL + PHP-FPM

### 2. Request Flow
Browser ──→ Nginx ──→ PHP-FPM ──→ MySQL ──→ Response
↑ ↓
└──────────────────←────────────────────────┘

text

### 3. Step-by-Step Flow

| Step | What Happens |
|------|--------------|
| 1 | Browser requests a page (e.g., index.php) |
| 2 | Nginx receives the request |
| 3 | Nginx sees it's a PHP file and passes it to PHP-FPM |
| 4 | PHP-FPM processes the PHP code |
| 5 | PHP-FPM queries MySQL if needed |
| 6 | MySQL returns data to PHP-FPM |
| 7 | PHP-FPM generates HTML and sends back to Nginx |
| 8 | Nginx sends HTML to browser |

### 4. LAMP vs LEMP - Web Server Model

| Aspect | LAMP (Apache) | LEMP (Nginx) |
|--------|---------------|--------------|
| Model | Process/thread-based | Event-driven |
| How it works | Creates new process/thread per connection | Single process handles many connections |
| Memory usage | Higher (each connection uses memory) | Lower (can handle 10k+ connections) |
| Best for | Dynamic content, .htaccess | Static files, high concurrency, reverse proxy |

**Simple explanation:**
- Apache = Creates a new waiter for each customer
- Nginx = One super-efficient waiter handling many customers at once

### 5. LAMP vs LEMP - PHP Execution

| Aspect | LAMP (Apache) | LEMP (Nginx) |
|--------|---------------|--------------|
| PHP handling | PHP module inside Apache (mod_php) | Separate PHP-FPM service |
| How it works | Apache runs PHP directly | Nginx passes PHP to PHP-FPM |
| Communication | Within same process | Via network (fastcgi) |
| Isolation | PHP runs as Apache user | PHP-FPM can run as different user |

**Simple explanation:**
- LAMP = Chef works in the same kitchen as waiter
- LEMP = Chef has separate kitchen, waiter brings orders

### 6. LAMP vs LEMP - Operational Tuning

| Aspect | LAMP | LEMP |
|--------|------|------|
| Configuration | .htaccess files per directory | Centralized nginx config |
| Performance tuning | Adjust MaxClients, KeepAlive | Adjust worker_processes, worker_connections |
| Static files | Served by Apache | Nginx serves them extremely fast |
| Reverse proxy | Possible but complex | Built-in, excellent performance |

### 7. LAMP vs LEMP - When to Choose Which

**Choose LAMP (Apache) When:**
- Using .htaccess files
- Apps require Apache-specific modules
- Team knows Apache well
- Shared hosting environment
- Easy directory-level config

**Choose LEMP (Nginx) When:**
- Need high concurrency (many users)
- Serving lots of static files
- Need reverse proxy/load balancer
- Modern PHP apps (Laravel, Symfony)
- Better performance with PHP-FPM

### 8. Nginx (Web Server)

- Event-driven, asynchronous architecture
- Handles thousands of connections with low memory
- Excellent for static files and reverse proxy
- Cannot process PHP directly – passes to PHP-FPM

### 9. PHP-FPM (FastCGI Process Manager)

- Separate service that processes PHP files
- Receives PHP requests from Nginx
- More efficient than mod_php for high traffic
- Can run with different user permissions

### 10. MySQL (Database)

- Stores data
- Same as in LAMP stack

## Commands Used

| Command | Purpose |
|---------|---------|
| `sudo apt install nginx mysql-server php-fpm php-mysql -y` | Install LEMP stack |
| `sudo systemctl status nginx` | Check Nginx status |
| `curl http://localhost | head -10` | Test Nginx web server |
| `sudo systemctl status mysql` | Check MySQL status |
| `php -v | head -1` | Find PHP version |
| `sudo systemctl status php8.3-fpm` | Check PHP-FPM service status |
| `sudo nano /var/www/html/info.php` | Create PHP test file |
| `curl http://localhost/info.php | head -20` | Test PHP via Nginx |

## Output / Evidence

### Environment Note

**This lab was completed in WSL (Windows Subsystem for Linux).** All LEMP components work correctly in WSL.

### Check Nginx

```bash
$ sudo systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-05-13 17:32:47 CEST; 13min ago
Check MySQL
bash
$ sudo systemctl status mysql
● mysql.service - MySQL Community Server
     Loaded: loaded (/usr/lib/systemd/system/mysql.service; enabled; preset: enabled)
     Active: active (running) since Fri 2026-03-13 20:22:56 CET; 15min ago
     Status: "Server is operational"
Check PHP-FPM
bash
$ php -v | head -1
PHP 8.3.6 (cli)

$ sudo systemctl status php8.3-fpm
● php8.3-fpm.service - The PHP 8.3 FastCGI Process Manager
     Loaded: loaded (/usr/lib/systemd/system/php8.3-fpm.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-05-13 17:33:07 CEST; 12min ago
     Status: "Processes active: 0, idle: 2, Requests: 0, slow: 0, Traffic: 0req/sec"
Configure Nginx to Use PHP-FPM
Create PHP test file:

bash
$ sudo nano /var/www/html/info.php
File content:

php
<?php
phpinfo();
?>
Test PHP via Nginx
bash
$ curl http://localhost/info.php | head -20
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"><head>
<style type="text/css">
body {background-color: #fff; color: #222; font-family: sans-serif;}
pre {margin: 0; font-family: monospace;}
a:link {color: #009; text-decoration: none; background-color: #fff;}
a:hover {text-decoration: underline;}
table {border-collapse: collapse; border: 0; width: 934px; box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.2);}
.center {text-align: center;}
.center table {margin: 1em auto; text-align: left;}

Validation

Component	Status	Command Used	Evidence
Nginx	✅ Working	sudo systemctl status nginx	active (running)
MySQL	✅ Working	sudo systemctl status mysql	active (running)
PHP-FPM	✅ Working	sudo systemctl status php8.3-fpm	active (running)
PHP processing	✅ Working	curl http://localhost/info.php	HTML output returned (not raw PHP code)
Troubleshooting
Scenario: PHP Not Processing (Raw Code Returned)
Problem: When visiting http://localhost/info.php, raw PHP code was returned instead of HTML output.

bash
$ curl http://localhost/info.php
<?php
phpinfo();
?>
Your Hypothesis: PHP not installed or not configured properly.

Commands to check:

bash
sudo systemctl status php8.3-fpm
ls /etc/nginx/sites-available/default
What went wrong: Nginx was not configured to pass PHP files to PHP-FPM. The nginx configuration was missing the PHP location block.

How to fix it:

bash
# Start PHP-FPM if not running
sudo systemctl start php8.3-fpm
sudo systemctl enable php8.3-fpm

# Ensure nginx config has PHP location block
sudo nano /etc/nginx/sites-available/default
Add this location block:

nginx
location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
}
Then reload nginx:

bash
sudo systemctl restart nginx
Verification:

bash
$ curl http://localhost/info.php | head -5
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN">
<html xmlns="http://www.w3.org/1999/xhtml"><head>
LEMP Stack Status Summary
Component	Status
Nginx	✅ Working
MySQL	✅ Working
PHP-FPM	✅ Working
PHP processing	✅ Working

Quiz Answers
Question	My Answer	Status
1. Expand LEMP	Linux Nginx MySQL PHP	✅ CORRECT
2. Nginx and PHP communicate via what component?	FPM (PHP-FPM)	✅ CORRECT
3. One operational difference vs LAMP	LEMP has only one server which caters all the requests	✅ CORRECT
4. Why tune worker/process settings carefully?	To optimize performance for your specific workload. Too few workers = slow response. Too many = waste memory.	✅ CORRECT
5. When choose LEMP?	When traffic is very much	✅ CORRECT
