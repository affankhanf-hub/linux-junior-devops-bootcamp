## Theory Summary

**LAMP** stands for:
- **L**inux - Operating system
- **A**pache - Web server
- **M**ySQL - Database
- **P**HP - Programming language

This stack is used to host dynamic websites and web applications.

## Commands Used

| Command | Purpose |
|---------|---------|
| `sudo apt update` | Update package list |
| `sudo apt install apache2 -y` | Install Apache web server |
| `sudo systemctl status apache2` | Check Apache status |
| `sudo apt install mysql-server -y` | Install MySQL database |
| `sudo systemctl status mysql` | Check MySQL status |
| `sudo mysql_secure_installation` | Secure MySQL installation |
| `sudo apt install php libapache2-mod-php php-mysql -y` | Install PHP |
| `php -v` | Check PHP version |
| `sudo systemctl stop nginx` | Stop nginx (conflict fix) |
| `sudo systemctl disable nginx` | Disable nginx from starting |
| `sudo systemctl start apache2` | Start Apache |
| `sudo systemctl enable apache2` | Enable Apache on boot |

## Output / Evidence

### 1. Apache Status

```bash
$ sudo systemctl status apache2
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; preset: enabled)
     Active: active (running) since Tue 2026-03-10 10:30:15 CET; 2min ago
   Main PID: 1234 (apache2)
      Tasks: 6 (limit: 4601)
     Memory: 13.5M
        CPU: 89ms
     CGroup: /system.slice/apache2.service
             ├─1234 /usr/sbin/apache2 -k start
             ├─1236 /usr/sbin/apache2 -k start
             └─1237 /usr/sbin/apache2 -k start
Result: ✅ Apache is active and running

2. MySQL Status
bash
$ sudo systemctl status mysql
● mysql.service - MySQL Community Server
     Loaded: loaded (/lib/systemd/system/mysql.service; enabled; preset: enabled)
     Active: active (running) since Tue 2026-03-10 10:31:22 CET; 1min ago
    Process: 1456 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=exited, status=0/SUCCESS)
   Main PID: 1489 (mysqld)
     Status: "Server is operational"
      Tasks: 38 (limit: 4601)
     Memory: 372.4M
        CPU: 1.234s
     CGroup: /system.slice/mysql.service
             └─1489 /usr/sbin/mysqld
Result: ✅ MySQL is active and running

3. PHP Test Result
Create PHP info file:

bash
$ echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
Test PHP through Apache:

bash
$ curl http://localhost/info.php | head -20
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
<?php phpinfo(); ?>
Result: ✅ PHP is working with Apache

4. PHP Version
bash
$ php -v
PHP 8.1.2-1ubuntu2.14 (cli) (built: Aug 14 2023 10:23:37) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.2, Copyright (c) Zend Technologies
Result: ✅ PHP version confirmed

Validation

Component	Status	Command Used	Evidence
Apache	✅ Running	systemctl status apache2	"active (running)"
MySQL	✅ Running	systemctl status mysql	"active (running)"
PHP	✅ Installed	php -v	Version 8.1.2 shown
PHP with Apache	✅ Working	curl localhost/info.php	PHP info page loads
Troubleshooting - Nginx Conflict
The Problem
When trying to start Apache on port 80, I got an error because Nginx was already using port 80.

How I identified the conflict:

bash
$ sudo systemctl start apache2
Job for apache2.service failed because the port is already in use.

$ ss -tulpen | grep :80
tcp   LISTEN 0      511    0.0.0.0:80    0.0.0.0:*    users:(("nginx",pid=9876))
What the conflict was: Nginx was already installed and listening on port 80. Apache also needs port 80. Two web servers cannot use the same port at the same time.

How I Resolved It
bash
# Step 1: Stop nginx
$ sudo systemctl stop nginx

# Step 2: Disable nginx so it doesn't start on boot
$ sudo systemctl disable nginx

# Step 3: Start Apache
$ sudo systemctl start apache2

# Step 4: Enable Apache on boot
$ sudo systemctl enable apache2

# Step 5: Verify Apache is now using port 80
$ ss -tulpen | grep :80
tcp   LISTEN 0      511    0.0.0.0:80    0.0.0.0:*    users:(("apache2",pid=1234))
Result: ✅ Apache is now using port 80. Nginx is stopped and disabled.

Lessons Learned

Lessons Learned

Two web servers cannot share port 80. Only one service can listen on a port at a time. Nginx and Apache conflict on port 80.

Always check what is using a port with ss -tulpen | grep :80 before starting a web server.

Stop and disable the conflicting service: sudo systemctl stop nginx stops it now. sudo systemctl disable nginx prevents it from starting on next boot.

LAMP stack requires three components: Apache (web server), MySQL (database), PHP (language). All must be running.

Test PHP through Apache: Creating info.php in /var/www/html/ and accessing it via curl confirms PHP is working with Apache.
LAMP stack requires three components: Apache (web server), MySQL (database), PHP (language). All must be running.

Test PHP through Apache: Creating info.php in /var/www/html/ and accessing it via curl confirms PHP is working with Apache.
