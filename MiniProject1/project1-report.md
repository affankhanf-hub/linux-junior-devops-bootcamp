Task 1 :Build Clean Ubuntu Baseline

Check your Ubuntu version

lsb_release -a

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.4 LTS
Release:	24.04
Codename:	noble
 
Check your kernel version

uname -a


affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ uname -a
Linux affan-khan-VirtualBox 6.17.0-14-generic #14~24.04.1-Ubuntu SMP PREEMPT_DYNAMIC Thu Jan 15 15:52:10 UTC 2 x86_64 x86_64 x86_64 GNU/Linux


TASK 2: Create Admin and App Users with Proper Permissions

Step 1: Create yourself (the admin)

# Create user affan

sudo adduser affan

Output:

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo adduser affan
info: Adding user `affan' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `affan' (1003) ...
info: Adding new user `affan' (1003) with group `affan (1003)' ...
info: Creating home directory `/home/affan' ...
info: Copying files from `/etc/skel' ...
New password: 
Retype new password: 
Sorry, passwords do not match.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: password updated successfully
Changing the user information for affan
Enter the new value, or press ENTER for the default
	Full Name []: Affan Naseem Khan
	Room Number []: 1
	Work Phone []: 03313733720
	Home Phone []: -
	Other []: -
Is the information correct? [Y/n] y
info: Adding new user `affan' to supplemental / extra groups `users' ...
info: Adding user `affan' to group `users' ...

Step 2: Make yourself an admin

sudo usermod -aG sudo affan

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo usermod -aG sudo affan
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ groups affan
affan : affan sudo users


Affan is the head

Adding Adil (Editor)

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo adduser adil
[sudo] password for affan-khan: 
info: Adding user `adil' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `adil' (1010) ...
info: Adding new user `adil' (1010) with group `adil (1010)' ...
info: Creating home directory `/home/adil' ...
info: Copying files from `/etc/skel' ...
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: password updated successfully
Changing the user information for adil
Enter the new value, or press ENTER for the default
	Full Name []: Adil
	Room Number []: 2
	Work Phone []: 9999999
	Home Phone []: -
	Other []: 
Is the information correct? [Y/n] y
info: Adding new user `adil' to supplemental / extra groups `users' ...
info: Adding user `adil' to group `users' ...


Adding Tahir (viewer)

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo adduser tahir
info: Adding user `tahir' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `tahir' (1014) ...
info: Adding new user `tahir' (1014) with group `tahir (1014)' ...
info: Creating home directory `/home/tahir' ...
info: Copying files from `/etc/skel' ...
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: password updated successfully
Changing the user information for tahir
Enter the new value, or press ENTER for the default
	Full Name []: Tahir
	Room Number []: 3
	Work Phone []: 5555555
	Home Phone []: -
	Other []: -
Is the information correct? [Y/n] y
info: Adding new user `tahir' to supplemental / extra groups `users' ...
info: Adding user `tahir' to group `users' ...


Create the website folder

sudo mkdir -p /var/www/company  (Nested folders)

Right now root owns this folder

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ ls -ld /var/www/company
drwxr-xr-x 2 root root 4096 Mar  5 22:57 /var/www/company


Create a group for website team

sudo groupadd companyteam

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo groupadd companyteam
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1


Changing ownership from root to group of folder


affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ ls-ld /var/www/company
ls-ld: command not found
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ ls - ld /var/www/company
ls: cannot access '-': No such file or directory
ls: cannot access 'ld': No such file or directory
/var/www/company:
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ ls -ld /var/www/company
drwxr-xr-x 2 root companyteam 4096 Mar  5 22:57 /var/www/company


Adding of members to group company team and verification


ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo usermod -aG companyteam  affan
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ ^C
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo usermod -aG companyteam  adil 
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo usermod -aG companyteam  tahir
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ group affan
Command 'group' not found, did you mean:
  command 'groups' from deb coreutils (9.4-3ubuntu6.1)
  command 'grop' from deb grop (2:0.10-1.2)
Try: sudo apt install <deb name>
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ groups affan
affan : affan sudo users companyteam
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ groups adil
adil : adil users companyteam
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ groups tahir
tahir : tahir users companyteam

Assigning permissions (770)

sudo chmod -R 770 /var/www/company

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo chmod -R 770 /var/www/company
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ ls -ld /var/www/company
drwxrwx--- 2 root companyteam 4096 Mar  5 22:57 /var/www/compan

Changing permission to 750 revoking write permission from group

ls -ld /var/www/company

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo chmod -R 750 /var/www/company
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ ls -ld var/www/company
ls: cannot access 'var/www/company': No such file or directory
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ ls -ld /var/www/company
drwxr-x--- 2 root companyteam 4096 Mar  5 22:57 /var/www/company

Task 3:

sudo apt update

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo apt update
[sudo] password for affan-khan: 
Hit:1 http://security.ubuntu.com/ubuntu noble-security InRelease
Hit:2 http://de.archive.ubuntu.com/ubuntu noble InRelease
Hit:3 http://de.archive.ubuntu.com/ubuntu noble-updates InRelease
Hit:4 http://de.archive.ubuntu.com/ubuntu noble-backports InRelease
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
12 packages can be upgraded. Run 'apt list --upgradable' to see them.

sudo apt install -y nginx

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ sudo apt install -y nginx
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
nginx is already the newest version (1.24.0-2ubuntu7.6).
0 upgraded, 0 newly installed, 0 to remove and 12 not upgraded.


Already installed

systemctl status nginx

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: en>
     Active: active (running) since Thu 2026-03-05 21:57:51 CET; 2h 43min ago
       Docs: man:nginx(8)
    Process: 1229 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_proce>
    Process: 1239 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (c>
   Main PID: 1252 (nginx)
      Tasks: 2 (limit: 4599)
     Memory: 2.9M (peak: 3.6M)
        CPU: 56ms
     CGroup: /system.slice/nginx.service
             ├─1252 "nginx: master process /usr/sbin/nginx -g daemon on; master>
             └─1253 "nginx: worker process"

Mar 05 21:57:50 affan-khan-VirtualBox systemd[1]: Starting nginx.service - A hi>
Mar 05 21:57:51 affan-khan-VirtualBox systemd[1]: Started nginx.service - A hig>

Step 4: Check if nginx starts at boot

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ systemctl is-enabled nginx
enabled


Step 5: Test if nginx is serving web pages

curl -I http://localhost

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Thu, 05 Mar 2026 23:44:32 GMT
Content-Type: text/html
Content-Length: 10671
Last-Modified: Wed, 19 Nov 2025 12:17:57 GMT
Connection: keep-alive
ETag: "691db575-29af"
Accept-Ranges: bytes


ss -tulpen | grep :80

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ ss -tulpen | grep :80
tcp   LISTEN 0      511          0.0.0.0:80         0.0.0.0:*    ino:11230 sk:b cgroup:/system.slice/nginx.service <->                       
tcp   LISTEN 0      511             [::]:80            [::]:*    ino:11231 sk:e cgroup:/system.slice/nginx.service v6only:1 <->              


curl http://localhost | head -10


ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ curl http://localhost | head -10
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
  <!--
    Modified from the Debian original for Ubuntu
    Last updated: 2022-03-22
    See: https://launchpad.net/bugs/1966004
  -->
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Apache2 Ubuntu Default Page: It works</title>
100 10671  100 10671    0     0  1781k      0 --:--:-- --:--:-- --:--:-- 2084k
curl: Failed writing body
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ ss -tulpen | grep :80^C
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ # Check nginx status
systemctl status nginx --no-pager | head -10

# Check apache2 status
systemctl status apache2 --no-pager | head -10

# Check which process actually has port 80
sudo lsof -i :80
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Thu 2026-03-05 21:57:51 CET; 2h 53min ago
       Docs: man:nginx(8)
    Process: 1229 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 1239 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 1252 (nginx)
      Tasks: 2 (limit: 4599)
     Memory: 3.2M (peak: 3.6M)
        CPU: 59ms
○ apache2.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/apache2.service; disabled; preset: enabled)
     Active: inactive (dead)
       Docs: https://httpd.apache.org/docs/2.4/
[sudo] password for affan-khan: 
COMMAND  PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
nginx   1252     root    5u  IPv4  11230      0t0  TCP *:http (LISTEN)
nginx   1252     root    6u  IPv6  11231      0t0  TCP *:http (LISTEN)
nginx   1253 www-data    5u  IPv4  11230      0t0  TCP *:http (LISTEN)
nginx   1253 www-data    6u  IPv6  11231      0t0  TCP *:http (LISTEN)
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/MiniProject1$ # For your evidence file
echo "=== Web Server: nginx ===" >> task3-evidence.

txt
systemctl status nginx --no-pager | head -15 >> task3-evidence.txt
systemctl is-enabled nginx >> task3-evidence.txt
curl -I http://localhost >> task3-evidence.txt
ss -tulpen | grep :80 >> task3-evidence.txt
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0 10671    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0


Task 4 : Create Maintenance Runbook and Rollback Notes

# Server Maintenance Runbook
## Project 1 - Linux Foundations Hardening

**Date:** March 6, 2026
**Server:** Ubuntu 24.04 LTS
**Author:** [Your Name]

---

## 1. SYSTEM INFORMATION

| Item | Details |
|------|---------|
| Operating System | Ubuntu 24.04 LTS |
| Web Server | nginx |
| Website Location | /var/www/company |
| Admin User | alex |
| App User | webuser |
| Group | companyteam |

---

## 2. USER ACCOUNTS

| Username | Role | Sudo Access | Can write to website? |
|----------|------|-------------|----------------------|
| Affan | Admin | ✅ Yes | ✅ Yes |
| Adil | Editor | ❌ No | ✅ Yes |
| Tahir | Viewer | ❌ No | ❌ No |

**To add a new user:**
```bash
sudo adduser username
sudo usermod -aG webteam username


sudo deluser username
sudo rm -rf /home/username

WEBSITE FOLDER PERMISSIONS

Location: /var/www/company

ls -ld /var/www/company

sudo chown -R root:webteam /var/www/company
sudo chmod -R 770 /var/www/company

DAILY MAINTENANCE TASKS
Task	Command	When
Check web server status	systemctl status nginx	Daily
Test website	curl -I http://localhost	Daily
Check disk space	df -h	Weekly
Check for errors	sudo journalctl -u nginx -n 20	Weekly

WEEKLY/UPDATING TASKS
Task	Command
Update package list	sudo apt update
Upgrade all packages	sudo apt upgrade -y
Remove unused packages	sudo apt autoremove -y

TROUBLESHOOTING GUIDE

Problem: Website not loading

Step 1: Check if nginx is running

systemctl status nginx


Step 2: Check logs for errors

sudo journalctl -u nginx --since "10 min ago"

Step 3: Check if port 80 is listening

ss -tulpen | grep :80


Step 4: Restart nginx

sudo systemctl restart nginx

Step 5: Test again


curl -I http://localhost

Problem: Permission denied when accessing website files

Step 1: Check current ownership

ls -ld /var/www/company

Step 2: Check your group membership

groups yourusername

Step 3: If you're not in webteam group, add yourself

sudo usermod -aG webteam yourusername
newgrp webteam

Step 4: Fix permissions if needed

sudo chown -R root:webteam /var/www/company
sudo chmod -R 770 /var/www/company

Problem: Can't install software

Step 1: Check internet connection
bash

ping -c 4 8.8.8.8

Step 2: Update package list
bash

sudo apt update

Step 3: Try install again
bash

sudo apt install -y packagename

7. ROLLBACK PLANS
If nginx update breaks the website:
bash

# Check what version you had
dpkg -l | grep nginx

# Restart nginx
sudo systemctl restart nginx

# If that doesn't work, reboot server
sudo reboot

If website files get deleted:
bash

# Create a backup first (for next time)
sudo tar -czf /backup/website-$(date +%Y%m%d).tar.gz /var/www/company

# If you have a backup, restore it
sudo tar -xzf /backup/website-20250306.tar.gz -C /

If user permissions are wrong:
bash

# Reset folder ownership
sudo chown -R root:webteam /var/www/company
sudo chmod -R 770 /var/www/company

# Re-add users to group
sudo usermod -aG webteam alex
sudo usermod -aG webteam sarah
sudo usermod -aG webteam mike

8. IMPORTANT FILE LOCATIONS
File/Directory	Purpose
/var/www/company	Website files
/etc/nginx/nginx.conf	Main nginx configuration
/etc/nginx/sites-available/	Site configurations
/var/log/nginx/access.log	Access logs
/var/log/nginx/error.log	Error logs
/home/	User home directories
/etc/passwd	User accounts
/etc/group	Group information
9. QUICK REFERENCE COMMANDS
What you want	Command
Check nginx status	systemctl status nginx
Restart nginx	sudo systemctl restart nginx
Test website	curl -I localhost
See nginx logs	sudo journalctl -u nginx -n 20
See who's on port 80	ss -tulpen | grep :80
Update system	sudo apt update && sudo apt upgrade -y
See all users	tail -5 /etc/passwd
See your groups	groups
See folder permissions	ls -ld /var/www/company
10. BACKUP PROCEDURE

Create a backup:
bash

# Create backup folder if it doesn't exist
sudo mkdir -p /backup

# Backup website files
sudo tar -czf /backup/company-$(date +%Y%m%d-%H%M%S).tar.gz /var/www/company

# Backup nginx config
sudo tar -czf /backup/nginx-config-$(date +%Y%m%d).tar.gz /etc/nginx/

List backups:
bash

ls -la /backup/

Restore from backup:
bash

sudo tar -xzf /backup/company-20250306-1430.tar.gz -C /

11. EMERGENCY CONTACTS
Issue	Contact
Server down	alex
Website hacked	alex
Can't login	alex
Unknown issue	Check logs first, then alex
12. NOTES

    Always run sudo apt update before installing new software

    Check logs first when something breaks

    Don't restart services blindly - investigate first

    Keep this runbook updated when you make changes

text


---

## STEP 3: SAVE AND EXIT

In nano:
- Press `Ctrl+O` to save
- Press `Enter` to confirm
- Press `Ctrl+X` to exit

---

## STEP 4: CREATE A SIMPLE VERSION (IF YOU WANT)

If the above is too long, here's a **shorter version**:

```bash
nano runbook-short.md

markdown

# Quick Runbook - Project 1

## System Info
- OS: Ubuntu 24.04
- Web Server: nginx
- Website: /var/www/company
- Users: alex (admin), sarah (editor), mike (viewer)
- Group: webteam

## Daily Checks

systemctl status nginx
curl -I localhost
text


## Common Fixes
**Website down:** 

systemctl restart nginx
journalctl -u nginx -n 20
text


**Permission issues:**

sudo chown -R root:webteam /var/www/company
sudo chmod -R 770 /var/www/company
text


## Important Files
- Website files: /var/www/company
- nginx config: /etc/nginx/nginx.conf
- Logs: `journalctl -u nginx`

## Quick Commands
| Task | Command |
|------|---------|
| Check nginx | `systemctl status nginx` |
| Test site | `curl -I localhost` |
| Check port | `ss -tulpen | grep :80` |
| Update | `sudo apt update && sudo apt upgrade -y` |


