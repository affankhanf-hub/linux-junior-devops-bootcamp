 Final Capstone Exam Project (Day 14)

1. Ubuntu host prep and baseline hardening

# 1.1 Check Ubuntu version
lsb_release -a

Output:
b_releaaffanlinux@DESKTOP-INM25AC:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.4 LTS
Release:        24.04
Codename:       noble

# 1.2 Check kernel
uname -a

Output:
affanlinux@DESKTOP-INM25AC:~$ uname -a
Linux DESKTOP-INM25AC 6.6.114.1-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Mon Dec  1 20:46:23 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux

# 1.3 Create admin user
sudo adduser capstone-admin

affanlinux@DESKTOP-INM25AC:~$ sudo adduser capstone-admin
[sudo] password for affanlinux:
Sorry, try again.
[sudo] password for affanlinux:
info: Adding user `capstone-admin' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `capstone-admin' (1006) ...
info: Adding new user `capstone-admin' (1006) with group `capstone-admin (1006)' ...
info: Creating home directory `/home/capstone-admin' ...
info: Copying files from `/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for capstone-admin
Enter the new value, or press ENTER for the default
        Full Name []: Affan Admin
        Room Number []: 1
        Work Phone []: 234
        Home Phone []:
        Other []:
Is the information correct? [Y/n] y
info: Adding new user `capstone-admin' to supplemental / extra groups `users' ...
info: Adding user `capstone-admin' to group `users' ...

# 1.4 Give sudo access
sudo usermod -aG sudo capstone-admin

ermod -aaffanlinux@DESKTOP-INM25AC:~$ sudo usermod -aG sudo capstone-admin

# 1.5 Secure sensitive files
sudo chmod 600 /etc/shadow
sudo chmod 600 /etc/gshadow
sudo chmod 644 /etc/passwd
sudo chmod 644 /etc/group

# 1.6 Enable automatic security updates

sudo apt update

Output:
Get:1 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]
Hit:2 http://archive.ubuntu.com/ubuntu noble InRelease
Get:3 http://archive.ubuntu.com/ubuntu noble-updates InRelease [126 kB]
Get:4 http://security.ubuntu.com/ubuntu noble-security/main amd64 Packages [1668 kB]
Get:5 http://archive.ubuntu.com/ubuntu noble-backports InRelease [126 kB]
Get:6 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 Packages [1969 kB]
Get:7 http://security.ubuntu.com/ubuntu noble-security/main Translation-en [264 kB]
Get:8 http://security.ubuntu.com/ubuntu noble-security/main amd64 Components [21.9 kB]
Get:9 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Packages [1187 kB]
Get:10 http://archive.ubuntu.com/ubuntu noble-updates/main Translation-en [351 kB]
Get:11 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 Components [178 kB]
Get:12 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 Packages [1689 kB]
Get:13 http://security.ubuntu.com/ubuntu noble-security/universe Translation-en [229 kB]
Get:14 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Components [74.1 kB]
Get:15 http://security.ubuntu.com/ubuntu noble-security/restricted amd64 Packages [2943 kB]
Get:16 http://archive.ubuntu.com/ubuntu noble-updates/universe Translation-en [328 kB]
Get:17 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 Components [386 kB]
Get:18 http://archive.ubuntu.com/ubuntu noble-updates/multiverse Translation-en [10.7 kB]
Get:19 http://archive.ubuntu.com/ubuntu noble-updates/multiverse amd64 Components [940 B]
Get:20 http://archive.ubuntu.com/ubuntu noble-backports/main amd64 Components [5776 B]
Get:21 http://archive.ubuntu.com/ubuntu noble-backports/universe amd64 Packages [31.0 kB]
Get:22 http://archive.ubuntu.com/ubuntu noble-backports/universe Translation-en [18.6 kB]
Get:23 http://archive.ubuntu.com/ubuntu noble-backports/universe amd64 Components [10.6 kB]
Get:24 http://security.ubuntu.com/ubuntu noble-security/restricted Translation-en [685 kB]
Get:25 http://security.ubuntu.com/ubuntu noble-security/multiverse Translation-en [7656 B]
Fetched 12.4 MB in 32s (393 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
10 packages can be upgraded. Run 'apt list --upgradable' to see them.


sudo apt install -y unattended-upgrades

Output:

affanlinux@DESKTOP-INM25AC:~$ sudo apt install -y unattended-upgrades
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
unattended-upgrades is already the newest version (2.9.1+nmu4ubuntu1).
unattended-upgrades set to manually installed.
0 upgraded, 0 newly installed, 0 to remove and 10 not upgraded.


sudo dpkg-reconfigure --priority=low unattended-upgrades

Output:


# 1.7 Verify

affanlinux@DESKTOP-INM25AC:~$ echo "=== HARDENING VERIFICATION ==="
=== HARDENING VERIFICATION ===
affanlinux@DESKTOP-INM25AC:~$ ls -la /etc/shadow
-rw------- 1 root shadow 1251 May 13 00:27 /etc/shadow
affanlinux@DESKTOP-INM25AC:~$ ls -la /etc/passwd
-rw-r--r-- 1 root root 1745 May 13 00:28 /etc/passwd



2. Networking setup verification (IP, gateway, DNS, routes)

# 1. Check your IP address
ip a

Output:

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:08:9b:3b brd ff:ff:ff:ff:ff:ff
    inet 172.25.169.7/20 brd 172.25.175.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::215:5dff:fe08:9b3b/64 scope link
       valid_lft forever preferred_lft forever

# 2. Check your gateway
ip route

Output:

affanlinux@DESKTOP-INM25AC:~$ ip route
default via 172.25.160.1 dev eth0 proto kernel
172.25.160.0/20 dev eth0 proto kernel scope link src 172.25.169.7

# 3. Check your DNS
cat /etc/resolv.conf

c/resolvaffanlinux@DESKTOP-INM25AC:~$ cat /etc/resolv.conf
# This file was automatically generated by WSL. To stop automatic generation of this file, add the following entry to /etc/wsl.conf:
# [network]
# generateResolvConf = false
nameserver 172.25.160.1


# 4. Test internet connection
ping -c 2 8.8.8.8

Output:

affanlinux@DESKTOP-INM25AC:~$ ping -c 2 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=23.3 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=118 time=13.7 ms

--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 13.665/18.457/23.250/4.792 ms

# 5. Test DNS resolution
ping -c 2 google.com

Output:
affanlinux@DESKTOP-INM25AC:~$ ping -c 2 google.com
PING google.com (142.251.13.138) 56(84) bytes of data.
64 bytes from wt-in-f138.1e100.net (142.251.13.138): icmp_seq=1 ttl=113 time=13.4 ms
64 bytes from wt-in-f138.1e100.net (142.251.13.138): icmp_seq=2 ttl=113 time=14.2 ms

--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 13.388/13.778/14.168/0.390 ms


Phase 3: Firewall Policy with Explicit Allow List

# 3.1 Set default policies

sudo ufw default deny incoming
affanlinux@DESKTOP-INM25AC:~$ sudo ufw default deny incoming
Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)


sudo ufw default allow outgoing

Output:

affanlinux@DESKTOP-INM25AC:~$ sudo ufw default allow outgoing
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)

# Explicit allow list

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

Output:
affanlinux@DESKTOP-INM25AC:~$ sudo ufw allow 22/tcp
do ufw allow 80/tcp
sudo ufw allow 443/tcpsudo ufw allow 80/tcp
sudo ufw allow 443/tcpRules updated
Rules updated (v6)

# Enable firewall
sudo ufw enable
Output:
affanlinux@DESKTOP-INM25AC:~$ sudo ufw enable
Firewall is active and enabled on system startup

# Verify
sudo ufw status verbose

Output:
affanlinux@DESKTOP-INM25AC:~$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)


Phase 4: Deploy LEMP Stack

# 4.1 Install LEMP packages
sudo apt update
sudo apt install -y nginx php-fpm php-mysql php-cli mariadb-server

Output:
Setting up mariadb-plugin-provider-bzip2 (1:10.11.14-0ubuntu0.24.04.1) ...
Setting up mariadb-plugin-provider-lzma (1:10.11.14-0ubuntu0.24.04.1) ...
Setting up php-fpm (2:8.3+93ubuntu2) ...
Setting up mariadb-plugin-provider-lzo (1:10.11.14-0ubuntu0.24.04.1) ...
Setting up mariadb-plugin-provider-lz4 (1:10.11.14-0ubuntu0.24.04.1) ...
Setting up libcgi-fast-perl (1:2.17-1) ...
Setting up mariadb-plugin-provider-snappy (1:10.11.14-0ubuntu0.24.04.1) ...
Processing triggers for ufw (0.36.2-6) ...
Processing triggers for man-db (2.12.0-4build2) ...
Processing triggers for libc-bin (2.39-0ubuntu8.7) ...
Processing triggers for php8.3-cli (8.3.6-0ubuntu0.24.04.8) ...
Processing triggers for php8.3-fpm (8.3.6-0ubuntu0.24.04.8) ...
Processing triggers for mariadb-server (1:10.11.14-0ubuntu0.24.04.1) ...

# 4.2 Find PHP version
PHP_VERSION=$(php -v | head -1 | cut -d' ' -f2 | cut -d'.' -f1,2)
echo "PHP Version: $PHP_VERSION"

Output:
affanlinux@DESKTOP-INM25AC:~$ # 4.2 Find PHP version
affanlinux@DESKTOP-INM25AC:~$ PHP_VERSION=$(php -v | head -1 | cut -d' ' -f2 | cut -d'.' -f1,2)
cho "PHP Version: $PHP_VERSION"affanlinux@DESKTOP-INM25AC:~$ echo "PHP Version: $PHP_VERSION"
PHP Version: 8.3

# 4.3 Start PHP-FPM
sudo systemctl start php$PHP_VERSION-fpm
sudo systemctl enable php$PHP_VERSION-fpm

Output:
affanlinux@DESKTOP-INM25AC:~$ # 4.3 Start PHP-FPM
affanlinux@DESKTOP-INM25AC:~$ sudo systemctl start php$PHP_VERSION-fpm
stemctl enable php$PHP_VERSION-fpmsudo systemctl enable php$PHP_VERSION-fpm

# 4.4 Create nginx configuration with PHP support

affanlinux@DESKTOP-INM25AC:~$ sudo tee /etc/nginx/sites-available/default > /dev/null << EOF
ver {
 > server {
>     listen 80 default_server;
>     root /var/www/html;
>     index index.php index.html;
   locat>
>     location / {
>         try_files \$uri \$uri/ =404;

    loc>     }
>
>     location ~ \.php\$ {
>         include snippets/fastcgi-php.conf;
>         fastcgi_pass unix:/var/run/php/php$PHP_VERSION-fpm.sock;
>     }
> }
> EOF

# 4.5 Test and reload nginx
sudo nginx -t
sudo systemctl reload nginx
affanlinux@DESKTOP-INM25AC:~$
affanlinux@DESKTOP-INM25AC:~$ # 4.5 Test and reload nginx
affanlinux@DESKTOP-INM25AC:~$ sudo nginx -t
sudo systemctl reload nginx
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

# 4.6 Create PHP test file
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.ph

affanlinux@DESKTOP-INM25AC:~$ echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.ph
<?php phpinfo(); ?>

# 4.7 Start MySQL
sudo systemctl start mariadb
sudo systemctl enable mariadb

# 4.8 Verify all services
echo "=== SERVICE STATUS ==="
sudo systemctl status nginx --no-pager | head -3
sudo systemctl status php$PHP_VERSION-fpm --no-pager | head -3
sudo systemctl status mariadb --no-pager | head -3

Output:
affanlinux@DESKTOP-INM25AC:~$ sudo systemctl status nginx --no-pager | head -3
systemctl status php$PHP_VERSION-fpm --no-pager | head -3
sudo systemctl status mariadb --no-pager | head -3● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-05-13 17:32:47 CEST; 13min ago
affanlinux@DESKTOP-INM25AC:~$ sudo systemctl status php$PHP_VERSION-fpm --no-pager | head -3
● php8.3-fpm.service - The PHP 8.3 FastCGI Process Manager
     Loaded: loaded (/usr/lib/systemd/system/php8.3-fpm.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-05-13 17:33:07 CEST; 12min ago

# 4.9 Test PHP execution
echo "=== PHP TEST ==="
curl http://localhost/info.php 2>/dev/null | head -5

Output:

affanlinux@DESKTOP-INM25AC:~$ sudo systemctl start nginx
systemctl status nginx --no-pager | head -5
curl http://localhost/info.php | head -5sudo systemctl status nginx --no-pager | head -5
curl http://localhost/info.php | head -5affanlinux@DESKTOP-INM25AC:~$
affanlinux@DESKTOP-INM25AC:~$ curl http://localhost/info.php 2>/dev/null | head -10
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


Phase 5: Deploy Static HTML App in Docker

affanlinux@DESKTOP-INM25AC:~/capstone-docker$ curl http://localhost:8080
<!DOCTYPE html>

<html<head>
    <title>Capstone Project - Static Site</title>
    <style>
        body { font-family: Arial; text-align: center; margin-top: 50px; }
        h1 { color: #2c3e50; }
        .container { border: 2px solid #3498db; padding: 20px; width: 80%; margin: 0 auto; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Welcome to Capstone Project</h1>
        <p>This static website is running in a Docker container on WSL.</p>
        <p>Container: nginx:alpine</p>
        <p>Port: 8080</p>
    </div>
</body>
</html>
affanlinux@DESKTOP-INM25AC:~/capstone-docker$ docker ps
ttp://localhost:8080CONTAINER ID   IMAGE               COMMAND                  CREATED              STATUS              PORTS                                     NAMES
c9193cf27e82   capstone-site:1.0   "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   capstone-container




Phase 6: Compare VM Service vs Container Service

affanlinux@DESKTOP-INM25AC:~$ curl http://localhost/info.php 2>/dev/null | head -5
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"><head>
<style type="text/css">
body {background-color: #fff; color: #222; font-family: sans-serif;}
pre {margin: 0; font-family: monospace;}
affanlinux@DESKTOP-INM25AC:~$ /html>
ux@DE-bash: syntax error near unexpected token `newline'
affanlinux@DESKTOP-INM25AC:~$ affanlinux@DESKTOP-INM25AC:~$
fanlinux@DESKTOP-INM25AC:~$ echo ""

affanlinux@DESKTOP-INM25AC:~$ echo "=== PHP TEST ==="
=== PHP TEST ===
affanlinux@DESKTOP-INM25AC:~$ curl http://localhost/info.php 2>/dev/null | head -5
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"><head>
<style type="text/css">
body {background-color: #fff; color: #222; font-family: sans-serif;}
pre {margin: 0; font-family: monospace;}affanlinux@DESKTOP-INM25AC:~$: command not found
affanlinux@DESKTOP-INM25AC:~$ affanlinux@DESKTOP-INM25AC:~$ echo ""
affanlinux@DESKTOP-INM25AC:~$: command not found
affanlinux@DESKTOP-INM25AC:~$
affanlinux@DESKTOP-INM25AC:~$ affanlinux@DESKTOP-INM25AC:~$ echo "=== PHP TEST ==="
affanlinux@DESKTOP-INM25AC:~$: command not found
affanlinux@DESKTOP-INM25AC:~$ === PHP TEST ===
===: command not found
affanlinux@DESKTOP-INM25AC:~$ affanlinux@DESKTOP-INM25AC:~$ curl http://localhost/info.php 2>/dev/null | head -5
affanlinux@DESKTOP-INM25AC:~$ <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "DTD/xhtml1-transitional.dtd">
-bash: !DOCTYPE: event not found
affanlinux@DESKTOP-INM25AC:~$ <html xmlns="http://www.w3.org/1999/xhtml"><head>
-bash: syntax error near unexpected token `<'
affanlinux@DESKTOP-INM25AC:~$ <style type="text/css">
-bash: syntax error near unexpected token `newline'
affanlinux@DESKTOP-INM25AC:~$ body {background-color: #fff; color: #222; font-family: sans-serif;}
Command 'body' not found, did you mean:
  command 'bdy' from deb geomview (1.9.5-4)
Try: sudo apt install <deb name>
affanlinux@DESKTOP-INM25AC:~$ pre {margin: 0; font-family: monospace;}



