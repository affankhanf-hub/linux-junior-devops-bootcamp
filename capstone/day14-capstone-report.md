markdown
# Day 14: Final Capstone Exam Project

## Architecture Diagram
┌─────────────────────────────────────────────────────────────────────────────┐
│ WSL (Windows Subsystem for Linux) │
│ │
│ ┌─────────────────────────────────────────────────────────────────────┐ │
│ │ Ubuntu 24.04 LTS │ │
│ │ │ │
│ │ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ │ │
│ │ │ Hardening │ │ Networking │ │ Firewall │ │ │
│ │ │ - Admin user│ │ - IP: WSL │ │ - UFW │ │ │
│ │ │ - File perms│ │ - Gateway │ │ - Ports: │ │ │
│ │ │ - Auto │ │ - DNS │ │ 22,80,443 │ │ │
│ │ │ updates │ │ - Internet │ │ │ │ │
│ │ └──────────────┘ └──────────────┘ └──────────────┘ │ │
│ │ │ │
│ │ ┌──────────────────────────────┐ ┌──────────────────────────────┐ │ │
│ │ │ LEMP Stack │ │ Docker Container │ │ │
│ │ │ ┌────┐ ┌────┐ ┌────┐ ┌────┐ │ │ ┌────────────────────────┐ │ │ │
│ │ │ │Nginx│ │PHP │ │MySQL│ │ │ │ │ nginx:alpine │ │ │ │
│ │ │ │:80 │ │-FPM│ │ │ │ │ │ │ Port 8080 │ │ │ │
│ │ │ └────┘ └────┘ └────┘ └────┘ │ │ │ Static HTML │ │ │ │
│ │ └──────────────────────────────┘ │ └────────────────────────┘ │ │ │
│ │ └──────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘

text

## Assumptions

| Assumption | Status |
|------------|--------|
| WSL2 with Ubuntu 24.04 LTS | ✅ Validated |
| User has sudo privileges | ✅ Validated |
| Internet connectivity | ✅ Validated |
| Docker installed | ✅ Validated |
| Ports 22, 80, 443, 8080 available | ✅ Validated |

---

## Phase 1: Ubuntu Host Prep and Baseline Hardening

### 1.1 Check Ubuntu Version
```bash
$ lsb_release -a
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.4 LTS
Release:        24.04
Codename:       noble
1.2 Check Kernel Version
bash
$ uname -a
Linux DESKTOP-INM25AC 6.6.114.1-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Mon Dec 1 20:46:23 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
1.3 Create Admin User
bash
$ sudo adduser capstone-admin
info: Adding user `capstone-admin' ...
info: Adding new user `capstone-admin' (1006) with group `capstone-admin (1006)' ...
Full Name []: Affan Admin
Is the information correct? [Y/n] y
1.4 Grant Sudo Access
bash
$ sudo usermod -aG sudo capstone-admin
1.5 Secure Sensitive Files
bash
$ sudo chmod 600 /etc/shadow
$ sudo chmod 600 /etc/gshadow
$ sudo chmod 644 /etc/passwd
$ sudo chmod 644 /etc/group
1.6 Enable Automatic Security Updates
bash
$ sudo apt update
Fetched 12.4 MB in 32s (393 kB/s)
Reading package lists... Done

$ sudo apt install -y unattended-upgrades
unattended-upgrades is already the newest version (2.9.1+nmu4ubuntu1)
1.7 Hardening Verification
bash
$ ls -la /etc/shadow
-rw------- 1 root shadow 1251 May 13 00:27 /etc/shadow

$ ls -la /etc/passwd
-rw-r--r-- 1 root root 1745 May 13 00:28 /etc/passwd
Result: ✅ Phase 1 Complete - System hardened, admin user created, automatic updates configured.

Phase 2: Networking Setup Verification
2.1 Check IP Address
bash
$ ip a
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    inet 172.25.169.7/20 brd 172.25.175.255 scope global eth0
2.2 Check Gateway
bash
$ ip route
default via 172.25.160.1 dev eth0 proto kernel
172.25.160.0/20 dev eth0 proto kernel scope link src 172.25.169.7
2.3 Check DNS
bash
$ cat /etc/resolv.conf
nameserver 172.25.160.1
2.4 Test Internet Connectivity
bash
$ ping -c 2 8.8.8.8
2 packets transmitted, 2 received, 0% packet loss
2.5 Test DNS Resolution
bash
$ ping -c 2 google.com
2 packets transmitted, 2 received, 0% packet loss
Result: ✅ Phase 2 Complete - Networking verified, IP: 172.25.169.7, Gateway: 172.25.160.1, DNS working.

Phase 3: Firewall Policy with Explicit Allow List
3.1 Set Default Policies
bash
$ sudo ufw default deny incoming
Default incoming policy changed to 'deny'

$ sudo ufw default allow outgoing
Default outgoing policy changed to 'allow'
3.2 Explicit Allow List
bash
$ sudo ufw allow 22/tcp
$ sudo ufw allow 80/tcp
$ sudo ufw allow 443/tcp
Rules updated
3.3 Enable Firewall
bash
$ sudo ufw enable
Firewall is active and enabled on system startup
3.4 Verify Firewall Status
bash
$ sudo ufw status verbose
Status: active
Default: deny (incoming), allow (outgoing)
To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
443/tcp                    ALLOW IN    Anywhere
Result: ✅ Phase 3 Complete - Firewall active with explicit allow list for ports 22, 80, 443.

Phase 4: Deploy LEMP Stack
4.1 Install LEMP Packages
bash
$ sudo apt update
$ sudo apt install -y nginx php-fpm php-mysql php-cli mariadb-server
4.2 Verify PHP Version
bash
$ php -v
PHP 8.3.6 (cli)
4.3 Start PHP-FPM
bash
$ sudo systemctl start php8.3-fpm
$ sudo systemctl enable php8.3-fpm
4.4 Configure Nginx for PHP
bash
$ sudo tee /etc/nginx/sites-available/default > /dev/null << 'EOF'
server {
    listen 80 default_server;
    root /var/www/html;
    index index.php index.html;
    location / {
        try_files $uri $uri/ =404;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
    }
}
EOF
4.5 Test and Reload Nginx
bash
$ sudo nginx -t
nginx: configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

$ sudo systemctl reload nginx
4.6 Create PHP Test File
bash
$ echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
4.7 Start MySQL
bash
$ sudo systemctl start mariadb
$ sudo systemctl enable mariadb
4.8 Verify Services
bash
$ sudo systemctl status nginx --no-pager | head -3
● nginx.service - active (running)

$ sudo systemctl status php8.3-fpm --no-pager | head -3
● php8.3-fpm.service - active (running)

$ sudo systemctl status mariadb --no-pager | head -3
● mariadb.service - active (running)
4.9 Test PHP Execution
bash
$ curl http://localhost/info.php 2>/dev/null | head -5
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN">
<html xmlns="http://www.w3.org/1999/xhtml"><head>
<style type="text/css">
body {background-color: #fff; color: #222; font-family: sans-serif;}
Result: ✅ Phase 4 Complete - LEMP stack deployed, PHP processing correctly (HTML output returned).

Phase 5: Deploy Static HTML App in Docker
5.1 Create Docker Files
bash
$ mkdir -p ~/capstone-docker
$ cat > ~/capstone-docker/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>Capstone Project - Static Site</title>
</head>
<body>
    <h1>Welcome to Capstone Project</h1>
    <p>This static website is running in Docker on WSL.</p>
</body>
</html>
EOF

$ cat > ~/capstone-docker/Dockerfile << 'EOF'
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
EOF
5.2 Build Docker Image
bash
$ cd ~/capstone-docker
$ docker build -t capstone-site:1.0 .
Successfully built e10a7de97b7f
Successfully tagged capstone-site:1.0
5.3 Run Docker Container
bash
$ docker run -d --name capstone-container -p 8080:80 capstone-site:1.0
c9193cf27e823978147df0ccf8f027fa3170eb44e839eec651c6e3c5ffd685a7
5.4 Verify Container Status
bash
$ docker ps
CONTAINER ID   IMAGE               STATUS         PORTS                                     NAMES
c9193cf27e82   capstone-site:1.0   Up 2 minutes   0.0.0.0:8080->80/tcp                      capstone-container
5.5 Test Docker Container
bash
$ curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
    <title>Capstone Project - Static Site</title>
</head>
<body>
    <h1>Welcome to Capstone Project</h1>
    <p>This static website is running in a Docker container on WSL.</p>
</body>
</html>
Result: ✅ Phase 5 Complete - Docker container running static HTML on port 8080.

Phase 6: Compare VM Service vs Container Service
Service Comparison Table
Aspect	VM Service (Host nginx)	Container Service (Docker nginx)
Port	80	8080
Access URL	http://localhost	http://localhost:8080
Process	nginx (systemd)	docker-proxy + container
Configuration Location	/etc/nginx/nginx.conf	Inside container image
Document Root	/var/www/html/	/usr/share/nginx/html/
Startup Behavior	Auto at boot (systemctl enable)	Manual (docker start)
Logs	journalctl -u nginx	docker logs capstone-container
Restart Command	sudo systemctl restart nginx	docker restart capstone-container
Resource Isolation	Shares host kernel	Namespace isolation
HTTP Response	200 OK (HTML output)	200 OK (HTML output)
PHP Test (Host)
bash
$ curl http://localhost/info.php 2>/dev/null | head -5
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN">
<html xmlns="http://www.w3.org/1999/xhtml"><head>
Docker Test (Container)
bash
$ curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
    <title>Capstone Project - Static Site</title>
Result: ✅ Phase 6 Complete - Both services working, comparison documented.

Phase 7: Resolve 3 Realistic Failures
Failure 1: PHP Not Processing (Raw Code Returned)
Step	Description
Symptom	curl http://localhost/info.php returned <?php phpinfo(); ?>
Root Cause	Nginx config missing PHP location block
Diagnosis	cat /etc/nginx/sites-available/default | grep -A5 "\.php" showed no output
Fix	Added location ~ \.php$ block to nginx config
Verification	curl returned HTML instead of raw PHP code
Prevention	Always include PHP location block when configuring nginx for PHP
Failure 2: Docker Permission Denied
Step	Description
Symptom	docker ps returned permission denied
Root Cause	User not in docker group
Diagnosis	groups did not show docker in output
Fix	sudo usermod -aG docker $USER + reopen WSL
Verification	docker ps showed running containers
Prevention	After installing Docker, always add user to docker group
Failure 3: Nginx 403 Forbidden
Step	Description
Symptom	curl http://localhost returned HTTP/1.1 403 Forbidden
Root Cause	Wrong ownership of /var/www/html (root owned, nginx runs as www-data)
Diagnosis	ls -ld /var/www/html showed root root; ps aux | grep nginx showed www-data
Fix	sudo chown -R www-data:www-data /var/www/html
Verification	curl http://localhost returned Test Page
Prevention	Always set correct ownership on web directories
