# Mini Project 1: Linux Foundations Hardening

## Architecture Diagram
┌─────────────────────────────────────────────────────────────────────────┐
│ Ubuntu 24.04 LTS Server │
│ │
│ ┌─────────────────────────────────────────────────────────────────┐ │
│ │ /var/www/company │ │
│ │ Permission: 750 (rwxr-x---) │ │
│ │ Owner: root, Group: companyteam │ │
│ │ │ │
│ │ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │ │
│ │ │ Affan │ │ Adil │ │ Tahir │ │ │
│ │ │ (Admin) │ │ (Editor) │ │ (Viewer) │ │ │
│ │ │ UID:1003 │ │ UID:1010 │ │ UID:1014 │ │ │
│ │ │ Group: │ │ Group: │ │ Group: │ │ │
│ │ │ companyteam│ │ companyteam│ │ companyteam│ │ │
│ │ └──────┬──────┘ └──────┬──────┘ └──────┬──────┘ │ │
│ │ │ │ │ │ │
│ │ ▼ ▼ ▼ │ │
│ │ ┌─────────────────────────────────────────────────────────┐ │ │
│ │ │ companyteam Group │ │ │
│ │ │ Permission on /var/www/company: 5 (r-x) │ │ │
│ │ └─────────────────────────────────────────────────────────┘ │ │
│ │ │ │ │
│ │ ▼ │ │
│ │ ┌─────────────────────────────────────────────────────────┐ │ │
│ │ │ Folder Access │ │ │
│ │ │ Affan: Read + Write + Execute (7) │ │ │
│ │ │ Adil: Read + Execute (5) - NO WRITE │ │ │
│ │ │ Tahir: Read + Execute (5) - NO WRITE │ │ │
│ │ └─────────────────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────────────────────┘ │
│ │
│ ┌─────────────────────────────────────────────────────────────────┐ │
│ │ Web Server │ │
│ │ nginx (Port 80) │ │
│ │ Status: active (running) │ │
│ └─────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────┘

text

## Task 1: Build Clean Ubuntu Baseline

**Check Ubuntu version:**
```bash
$ lsb_release -a
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.4 LTS
Release:        24.04
Codename:       noble
Check kernel version:

bash
$ uname -a
Linux affan-khan-VirtualBox 6.17.0-14-generic #14~24.04.1-Ubuntu SMP PREEMPT_DYNAMIC Thu Jan 15 15:52:10 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
Result: ✅ Ubuntu 24.04.4 LTS with kernel 6.17.0-14

Task 2: Create Admin and App Users with Proper Permissions

Create Admin User (Affan)
bash
$ sudo adduser affan
info: Adding user `affan' ...
info: Adding new user `affan' (1003) with group `affan (1003)' ...
New password: 
Retype new password: 
Full Name []: Affan Naseem Khan
Is the information correct? [Y/n] y
Make Affan Admin
bash
$ sudo usermod -aG sudo affan
$ groups affan
affan : affan sudo users
Create Editor User (Adil)
bash
$ sudo adduser adil
info: Adding user `adil' (1010) with group `adil (1010)' ...

Full Name []: Adil
Create Viewer User (Tahir)
bash
$ sudo adduser tahir
info: Adding user `tahir' (1014) with group `tahir (1014)' ...

Full Name []: Tahir
Create Website Folder and Group
bash
$ sudo mkdir -p /var/www/company
$ sudo groupadd companyteam
Change Folder Ownership to Group
bash
$ sudo chown -R root:companyteam /var/www/company
$ ls -ld /var/www/company
drwxr-xr-x 2 root companyteam 4096 Mar 5 22:57 /var/www/company
Add Users to Group
bash
$ sudo usermod -aG companyteam affan
$ sudo usermod -aG companyteam adil
$ sudo usermod -aG companyteam tahir

$ groups affan
affan : affan sudo users companyteam
$ groups adil
adil : adil users companyteam
$ groups tahir
tahir : tahir users companyteam
Set Permissions to 750
bash
$ sudo chmod -R 750 /var/www/company
$ ls -ld /var/www/company
drwxr-x--- 2 root companyteam 4096 Mar 5 22:57 /var/www/company
Role-Based Access Validation
User	Role	In companyteam?	Can Read?	Can Write?	Can Execute?
Affan	Admin	✅ Yes	✅ Yes	✅ Yes (via sudo)	✅ Yes
Adil	Editor	✅ Yes	✅ Yes	❌ No (750 restricts)	✅ Yes
Tahir	Viewer	✅ Yes	✅ Yes	❌ No (750 restricts)	✅ Yes
Other	Other	❌ No	❌ No	❌ No	❌ No
Why 750? Owner (root) has full control. Group (companyteam) can read and execute but NOT write. Others have no access.

Task 3: Install and Verify Nginx

Update Package List
bash
$ sudo apt update
Hit:1 http://security.ubuntu.com/ubuntu noble-security InRelease
Hit:2 http://de.archive.ubuntu.com/ubuntu noble InRelease
Reading package lists... Done
Install Nginx
bash
$ sudo apt install -y nginx
nginx is already the newest version (1.24.0-2ubuntu7.6)
Check Nginx Status
bash
$ systemctl status nginx
● nginx.service - A high performance web server
     Active: active (running) since Thu 2026-03-05 21:57:51 CET
   Main PID: 1252 (nginx)
Check Nginx Enabled on Boot
bash
$ systemctl is-enabled nginx
enabled
Test Nginx - HTTP Headers
bash
$ curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Content-Type: text/html
Check Port 80 Listening
bash
$ ss -tulpen | grep :80
tcp   LISTEN 0      511    0.0.0.0:80    0.0.0.0:*    users:(("nginx",pid=1252))
tcp   LISTEN 0      511       [::]:80       [::]:*    users:(("nginx",pid=1252))
Test Nginx - Full Page
bash
$ curl http://localhost | head -10
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
</head>
<body>
<h1>Welcome to nginx!</h1>
Result: ✅ Nginx is installed, running, enabled on boot, and serving web pages.

Task 4: Create Maintenance Runbook and Rollback Notes

Runbook Structure
Section	Purpose
1. System Information	Server details at a glance
2. User Accounts	Who has access and what they can do
3. Website Folder Permissions	Current permission settings
4. Daily Maintenance	Commands to run each day
5. Weekly Maintenance	Commands to run each week
6. Troubleshooting Guide	Step-by-step problem solving
7. Rollback Plans	How to undo changes
8. Important File Locations	Where configs and logs live
9. Quick Reference	Most common commands
10. Backup Procedure	How to backup and restore

Quick Runbook (Short Version)
markdown
# Quick Runbook - Project 1

## System Info
- OS: Ubuntu 24.04 LTS
- Web Server: nginx (Port 80)
- Website: /var/www/company
- Users: Affan (admin), Adil (editor), Tahir (viewer)
- Group: companyteam

## Role-Based Access
| User | Role | Can Write? |
|------|------|------------|
| Affan | Admin | ✅ Yes |
| Adil | Editor | ❌ No |
| Tahir | Viewer | ❌ No |

## Daily Checks
```bash
systemctl status nginx
curl -I http://localhost
Common Fixes
Website down:

bash
systemctl restart nginx
journalctl -u nginx -n 20
Permission issues:

bash
ls -ld /var/www/company
groups yourusername
sudo chmod -R 750 /var/www/company
Important Files
File	Purpose
/var/www/company	Website files
/etc/nginx/nginx.conf	nginx configuration
/var/log/nginx/error.log	Error logs
Quick Commands
Task	Command
Check nginx	systemctl status nginx
Test site	curl -I localhost
Check port	ss -tulpen | grep :80
View logs	journalctl -u nginx -n 20
Update system	sudo apt update && sudo apt upgrade -y
text

### Rollback Plan

| Scenario | Rollback Steps |
|----------|----------------|
| Nginx update breaks site | `sudo systemctl restart nginx` → Check logs → Reboot if needed |
| Website files deleted | Restore from backup: `sudo tar -xzf /backup/company-YYYYMMDD.tar.gz -C /` |
| Wrong permissions | `sudo chown -R root:companyteam /var/www/company` → `sudo chmod -R 750 /var/www/company` |

## Validation Summary

| Component | Status | Command | Evidence |
|-----------|--------|---------|----------|
| Ubuntu Version | ✅ | `lsb_release -a` | 24.04.4 LTS |
| Kernel Version | ✅ | `uname -a` | 6.17.0-14 |
| User Affan | ✅ | `groups affan` | sudo users companyteam |
| User Adil | ✅ | `groups adil` | adil users companyteam |
| User Tahir | ✅ | `groups tahir` | tahir users companyteam |
| Group companyteam | ✅ | `ls -ld /var/www/company` | root companyteam |
| Folder Permission | ✅ | `ls -ld /var/www/company` | drwxr-x--- (750) |
| Nginx Service | ✅ | `systemctl status nginx` | active (running) |
| Nginx Enabled | ✅ | `systemctl is-enabled nginx` | enabled |
| Port 80 | ✅ | `ss -tulpen | grep :80` | nginx listening |
| Web Page | ✅ | `curl -I http://localhost` | 200 OK |

## Lessons Learned

1. **Role-based access requires separate testing** - Each user (Affan, Adil, Tahir) needs to be tested individually to prove permissions work.

2. **750 permission means:** Owner (root) has full control (7), Group (companyteam) can read and execute but NOT write (5), Others have no access (0).

3. **Nginx runs on port 80** - Always verify with `ss -tulpen | grep :80` to ensure it is listening.

4. **Runbook should have clear sections** - System Info, Daily Checks, Common Fixes, Quick Commands make it usable in emergencies.

5. **Architecture diagram helps understanding** - A simple text diagram shows how users, group, and folder relate to each other.

6. **Validation must be explicit** - For each component, show the command and the output that proves success.
