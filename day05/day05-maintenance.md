10  points

1. Package management is how Linux installs, updates, and removes software safely from trusted repositories.

2. Sudo apt update refreshes the list of available packages from repositories – must run before installing anything.

3. Sudo apt install -y installs packages automatically without asking for confirmation (the -y flag answers "yes" for me).

4. You can install multiple packages at once like sudo apt install -y nginx apache2 ufw fail2ban.

5. Dependencies are other packages that software needs to work – apt handles them automatically.

6. sudo apt autoremove -y removes unused dependencies that were installed but are no longer needed – frees up disk space.

7. Baseline hardening means making your system more secure by:

 Patching the system (keeping software updated)
 Restricting SSH access
 Enforcing firewall defaults
 Removing unnecessary services

8. Sudo systemctl list-unit-files --type=service | grep enabled shows all services that start automatically at boot.

9. Every enabled service is a potential attack surface – disable what you don't need.

10. Ufw (Uncomplicated Firewall) should be set to default deny incoming, allow outgoing, and only open specific ports like 22 (SSH) and 80 (HTTP).



Lab Task 1: Command 1 – sudo apt update

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day05$ sudo apt update


Output: [sudo] password for affan-khan: 
Sorry, try again.
[sudo] password for affan-khan: 
Hit:1 http://de.archive.ubuntu.com/ubuntu noble InRelease
Get:2 http://de.archive.ubuntu.com/ubuntu noble-updates InRelease [126 kB]
Get:3 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]
Get:4 http://de.archive.ubuntu.com/ubuntu noble-backports InRelease [126 kB]
Get:5 http://de.archive.ubuntu.com/ubuntu noble-updates/main amd64 Packages [1,805 kB]
Get:6 http://de.archive.ubuntu.com/ubuntu noble-updates/main Translation-en [332 kB]
Get:7 http://de.archive.ubuntu.com/ubuntu noble-updates/main amd64 Components [177 kB]
Get:8 http://de.archive.ubuntu.com/ubuntu noble-updates/main amd64 c-n-f Metadata [16.7 kB]
Get:9 http://de.archive.ubuntu.com/ubuntu noble-updates/restricted amd64 Packages [2,747 kB]
Get:10 http://de.archive.ubuntu.com/ubuntu noble-updates/restricted Translation-en [632 kB]
Get:11 http://de.archive.ubuntu.com/ubuntu noble-updates/restricted amd64 Components [212 B]
Get:12 http://de.archive.ubuntu.com/ubuntu noble-updates/universe amd64 Packages [1,564 kB]
Get:13 http://security.ubuntu.com/ubuntu noble-security/main amd64 Packages [1,504 kB]
Get:14 http://de.archive.ubuntu.com/ubuntu noble-updates/universe Translation-en [318 kB]
Get:15 http://de.archive.ubuntu.com/ubuntu noble-updates/universe amd64 Components [386 kB]
Get:16 http://de.archive.ubuntu.com/ubuntu noble-updates/universe amd64 c-n-f Metadata [32.9 kB]
Get:17 http://de.archive.ubuntu.com/ubuntu noble-updates/multiverse amd64 Components [940 B]
Get:18 http://de.archive.ubuntu.com/ubuntu noble-backports/main amd64 Components [7,284 B]
Get:19 http://de.archive.ubuntu.com/ubuntu noble-backports/restricted amd64 Components [216 B]
Get:20 http://de.archive.ubuntu.com/ubuntu noble-backports/universe amd64 Components [10.5 kB]
Get:21 http://de.archive.ubuntu.com/ubuntu noble-backports/multiverse amd64 Components [212 B]
Get:22 http://security.ubuntu.com/ubuntu noble-security/main Translation-en [241 kB]
Get:23 http://security.ubuntu.com/ubuntu noble-security/main amd64 Components [21.5 kB]
Get:24 http://security.ubuntu.com/ubuntu noble-security/main amd64 c-n-f Metadata [10.1 kB]
Get:25 http://security.ubuntu.com/ubuntu noble-security/restricted amd64 Packages [2,599 kB]
Get:26 http://security.ubuntu.com/ubuntu noble-security/restricted Translation-en [600 kB]
Get:27 http://security.ubuntu.com/ubuntu noble-security/restricted amd64 Components [212 B]
Get:28 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Packages [975 kB]
et:29 http://security.ubuntu.com/ubuntu noble-security/universe Translation-en [218 kB]
Get:30 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Components [74.3 kB]
Get:31 http://security.ubuntu.com/ubuntu noble-security/universe amd64 c-n-f Metadata [20.6 kB]
Get:32 http://security.ubuntu.com/ubuntu noble-security/multiverse amd64 Components [212 B]
Fetched 14.7 MB in 11s (1,351 kB/s)                                            
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
12 packages can be upgraded. Run 'apt list --upgradable' to see them.


Lab Task 2: Command 2 – sudo apt install -y nginx apache2 ufw fail2ban

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day05$ sudo apt install -y nginx apache2 ufw fail2ban

Output:nstall -y nginx apache2 ufw fail2ban
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
nginx is already the newest version (1.24.0-2ubuntu7.6).
apache2 is already the newest version (2.4.58-1ubuntu8.10).
ufw is already the newest version (0.36.2-6).
ufw set to manually installed.
The following package was automatically installed and is no longer required:
  libllvm19
Use 'sudo apt autoremove' to remove it.
The following additional packages will be installed:
  python3-pyasyncore python3-pyinotify python3-setuptools whois
Suggested packages:
  mailx monit sqlite3 python-pyinotify-doc python-setuptools-doc
The following NEW packages will be installed:
  fail2ban python3-pyasyncore python3-pyinotify python3-setuptools whois
0 upgraded, 5 newly installed, 0 to remove and 12 not upgraded.
Need to get 892 kB of archives.
After this operation, 4,858 kB of additional disk space will be used.
Get:1 http://de.archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-setuptools all 68.1.2-2ubuntu1.2 [397 kB]
Get:2 http://de.archive.ubuntu.com/ubuntu noble/main amd64 python3-pyasyncore all 1.0.2-2 [10.1 kB]
Get:3 http://de.archive.ubuntu.com/ubuntu noble-updates/universe amd64 fail2ban all 1.0.2-3ubuntu0.1 [409 kB]
Get:4 http://de.archive.ubuntu.com/ubuntu noble/main amd64 python3-pyinotify all 0.9.6-2ubuntu1 [25.0 kB]
Get:5 http://de.archive.ubuntu.com/ubuntu noble/main amd64 whois amd64 5.5.22 [51.7 kB]
Fetched 892 kB in 4s (218 kB/s)
Selecting previously unselected package python3-setuptools.
(Reading database ... 198412 files and directories currently installed.)
Preparing to unpack .../python3-setuptools_68.1.2-2ubuntu1.2_all.deb ...
Unpacking python3-setuptools (68.1.2-2ubuntu1.2) ...
Selecting previously unselected package python3-pyasyncore.
Preparing to unpack .../python3-pyasyncore_1.0.2-2_all.deb ...
Unpacking python3-pyasyncore (1.0.2-2) ...
Selecting previously unselected package fail2ban.
Preparing to unpack .../fail2ban_1.0.2-3ubuntu0.1_all.deb ...
Unpacking fail2ban (1.0.2-3ubuntu0.1) ...
Selecting previously unselected package python3-pyinotify.
Preparing to unpack .../python3-pyinotify_0.9.6-2ubuntu1_all.deb ...
Unpacking python3-pyinotify (0.9.6-2ubuntu1) ...
Selecting previously unselected package whois.
Preparing to unpack .../whois_5.5.22_amd64.deb ...
Unpacking whois (5.5.22) ...
Setting up python3-setuptools (68.1.2-2ubuntu1.2) ...
Setting up whois (5.5.22) ...
Setting up python3-pyasyncore (1.0.2-2) ...
Setting up fail2ban (1.0.2-3ubuntu0.1) ...
/usr/lib/python3/dist-packages/fail2ban/tests/fail2banregextestcase.py:224: Synt
axWarning: invalid escape sequence '\s'
  "1490349000 test failed.dns.ch", "^\s*test <F-ID>\S+</F-ID>"
/usr/lib/python3/dist-packages/fail2ban/tests/fail2banregextestcase.py:435: Synt
axWarning: invalid escape sequence '\S'
  '^'+prefix+'<F-ID>User <F-USER>\S+</F-USER></F-ID> not allowed\n'
/usr/lib/python3/dist-packages/fail2ban/tests/fail2banregextestcase.py:443: Synt
axWarning: invalid escape sequence '\S'
  '^'+prefix+'User <F-USER>\S+</F-USER> not allowed\n'
/usr/lib/python3/dist-packages/fail2ban/tests/fail2banregextestcase.py:444: Synt
axWarning: invalid escape sequence '\d'
  '^'+prefix+'Received disconnect from <F-ID><ADDR> port \d+</F-ID>'
/usr/lib/python3/dist-packages/fail2ban/tests/fail2banregextestcase.py:451: Synt
axWarning: invalid escape sequence '\s'
  _test_variants('common', prefix="\s*\S+ sshd\[<F-MLFID>\d+</F-MLFID>\]:\s+")
/usr/lib/python3/dist-packages/fail2ban/tests/fail2banregextestcase.py:537: Synt
axWarning: invalid escape sequence '\['
  'common[prefregex="^svc\[<F-MLFID>\d+</F-MLFID>\] connect <F-CONTENT>.+</F-CON
TENT>$"'
/usr/lib/python3/dist-packages/fail2ban/tests/servertestcase.py:1375: SyntaxWarn
ing: invalid escape sequence '\s'
  "`{ nft -a list chain inet f2b-table f2b-chain | grep -oP '@addr-set-j-w-nft-m
p\s+.*\s+\Khandle\s+(\d+)$'; } | while read -r hdl; do`",
/usr/lib/python3/dist-packages/fail2ban/tests/servertestcase.py:1378: SyntaxWarn
ing: invalid escape sequence '\s'
  "`{ nft -a list chain inet f2b-table f2b-chain | grep -oP '@addr6-set-j-w-nft-
mp\s+.*\s+\Khandle\s+(\d+)$'; } | while read -r hdl; do`",
/usr/lib/python3/dist-packages/fail2ban/tests/servertestcase.py:1421: SyntaxWarn
ing: invalid escape sequence '\s'
  "`{ nft -a list chain inet f2b-table f2b-chain | grep -oP '@addr-set-j-w-nft-a
p\s+.*\s+\Khandle\s+(\d+)$'; } | while read -r hdl; do`",
/usr/lib/python3/dist-packages/fail2ban/tests/servertestcase.py:1424: SyntaxWarn
ing: invalid escape sequence '\s'
  "`{ nft -a list chain inet f2b-table f2b-chain | grep -oP '@addr6-set-j-w-nft-
ap\s+.*\s+\Khandle\s+(\d+)$'; } | while read -r hdl; do`",
Created symlink /etc/systemd/system/multi-user.target.wants/fail2ban.service → /
usr/lib/systemd/system/fail2ban.service.
Setting up python3-pyinotify (0.9.6-2ubuntu1) ...
Processing triggers for man-db (2.12.0-4build2) ...

Validation whether installed or not

dpkg -l | grep nginx

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day05$ dpkg -l | grep nginx
ii  nginx                                         1.24.0-2ubuntu7.6                        amd64        small, powerful, scalable web/proxy server
ii  nginx-common                                  1.24.0-2ubuntu7.6                        all          small, powerful, scalable web/proxy server - common files


Installed


dpkg -l | grep apache2

ii  apache2                                       2.4.58-1ubuntu8.10                       amd64        Apache HTTP Server
ii  apache2-bin                                   2.4.58-1ubuntu8.10                       amd64        Apache HTTP Server (modules and other binary files)
ii  apache2-data                                  2.4.58-1ubuntu8.10                       all          Apache HTTP Server (common files)
ii  apache2-utils                                 2.4.58-1ubuntu8.10                       amd64        Apache HTTP Server (utility programs for web servers)


Installed

dpkg -l | grep ufw

ii  ufw                                           0.36.2-6                                 all          program for managing a Netfilter firewall


Installed

dpkg -l | grep fail2ban

ii  fail2ban                                      1.0.2-3ubuntu0.1                         all          ban hosts that cause multiple authentication errors

Installed

Lab Task 3: Check What Was Installed

systemctl status nginx --no-pager | head -5
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-03-04 16:30:48 CET; 10h ago
       Docs: man:nginx(8)
   Main PID: 1259 (nginx)

same for other softwares

Lab Task 4: Command 3 – sudo apt autoremove -y

[sudo] password for affan-khan: 
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages will be REMOVED:
  libllvm19
0 upgraded, 0 newly installed, 1 to remove and 12 not upgraded.
After this operation, 129 MB disk space will be freed.
(Reading database ... 199109 files and directories currently installed.)
Removing libllvm19:amd64 (1:19.1.1-1ubuntu1~24.04.2) ...
Processing triggers for libc-bin (2.39-0ubuntu8.7) .

Lab Task 5: Command 4 – sudo systemctl list-unit-files --type=service | grep enabled


mctl list-unit-files --type=service | grep enabled
accounts-daemon.service                      enabled         enabled
alsa-utils.service                           masked          enabled
anacron.service                              enabled         enabled
apache-htcacheclean.service                  disabled        enabled
apache-htcacheclean@.service                 disabled        enabled
apache2.service                              disabled        enabled
apache2@.service                             disabled        enabled
apparmor.service                             enabled         enabled
apport.service                               enabled         enabled
avahi-daemon.service                         enabled         enabled
bluetooth.service                            enabled         enabled
brltty.service                               disabled        enabled
cloud-config.service                         enabled         enabled
cloud-final.service                          enabled         enabled
cloud-init-local.service                     enabled         enabled
cloud-init.service                           enabled         enabled
console-setup.service                        enabled         enabled
cron.service                                 enabled         enabled
cryptdisks-early.service                     masked          enabled
cryptdisks.service                           masked          enabled
cups-browsed.service                         enabled         enabled
cups.service                                 enabled         enabled
dmesg.service                                enabled         enabled
e2scrub_reap.service                         enabled         enabled
fail2ban.service                             enabled         enabled
getty@.service                               enabled         enabled
gnome-remote-desktop.service                 enabled         enabled
gpu-manager.service                          enabled         enabled
grub-common.service                          enabled         enabled
grub-initrd-fallback.service                 enabled         enabled
hwclock.service                              masked          enabled
kerneloops.service                           enabled         enabled
keyboard-setup.service                       enabled         enabled
ModemManager.service                         enabled         enabled
netplan-ovs-cleanup.service                  enabled-runtime enabled
networkd-dispatcher.service                  enabled         enabled
NetworkManager-dispatcher.service            enabled         enabled
NetworkManager-wait-online.service           enabled         enabled
NetworkManager.service                       enabled         enabled
nftables.service                             disabled        enabled
nginx.service                                enabled         enabled
open-vm-tools.service                        enabled         enabled
openvpn-client@.service                      disabled        enabled
openvpn-server@.service                      disabled        enabled
openvpn.service                              enabled         enabled
openvpn@.service                             disabled        enabled
power-profiles-daemon.service                enabled         enabled
rsync.service                                disabled        enabled
rsyslog.service                              enabled         enabled
rtkit-daemon.service                         disabled        enabled
saned.service                                masked          enabled
saned@.service                               indirect        enabled
secureboot-db.service                        enabled         enabled
serial-getty@.service                        disabled        enabled
setvtrgb.service                             enabled         enabled
snapd.apparmor.service                       enabled         enabled
snapd.autoimport.service                     enabled         enabled
snapd.core-fixup.service                     enabled         enabled
snapd.recovery-chooser-trigger.service       enabled         enabled
snapd.seeded.service                         enabled         enabled
snapd.service                                enabled         enabled
snapd.system-shutdown.service                enabled         enabled
speech-dispatcherd.service                   disabled        enabled
spice-vdagentd.service                       indirect        enabled
ssh.service                                  disabled        enabled
ssl-cert.service                             enabled         enabled
sssd-autofs.service                          indirect        enabled
sssd-nss.service                             indirect        enabled
sssd-pac.service                             indirect        enabled
sssd-pam.service                             indirect        enabled
sssd-ssh.service                             indirect        enabled
sssd-sudo.service                            indirect        enabled
sssd.service                                 enabled         enabled
sudo.service                                 masked          enabled
switcheroo-control.service                   enabled         enabled
sysstat.service                              enabled         enabled
systemd-confext.service                      disabled        enabled
systemd-fsck-root.service                    enabled-runtime enabled
systemd-network-generator.service            disabled        enabled
systemd-networkd-wait-online.service         disabled        enabled
systemd-networkd-wait-online@.service        disabled        enabled
systemd-networkd.service                     disabled        enabled
systemd-oomd.service                         enabled         enabled
systemd-pcrlock-file-system.service          disabled        enabled
systemd-pcrlock-firmware-code.service        disabled        enabled
systemd-pcrlock-firmware-config.service      disabled        enabled
systemd-pcrlock-machine-id.service           disabled        enabled
systemd-pcrlock-make-policy.service          disabled        enabled
systemd-pcrlock-secureboot-authority.service disabled        enabled
systemd-pcrlock-secureboot-policy.service    disabled        enabled
systemd-pstore.service                       enabled         enabled
systemd-remount-fs.service                   enabled-runtime enabled
systemd-resolved.service                     enabled         enabled
systemd-sysext.service                       disabled        enabled
systemd-sysupdate-reboot.service             indirect        enabled
systemd-sysupdate.service                    indirect        enabled
systemd-timesyncd.service                    enabled         enabled
thermald.service                             enabled         enabled
ua-reboot-cmds.service                       enabled         enabled
ubuntu-advantage.service                     enabled         enabled
udisks2.service                              enabled         enabled
ufw.service                                  enabled         enabled
unattended-upgrades.service                  enabled         enabled
upower.service                               disabled        enabled
uuidd.service                                indirect        enabled
vgauth.service                               enabled         enabled
wpa_supplicant-nl80211@.service              disabled        enabled
wpa_supplicant-wired@.service                disabled        enabled
wpa_supplicant.service                       enabled         enabled
wpa_supplicant@.service                      disabled        enabled
x11-common.service                           masked          enabled


Command Flags  Command 2: sudo apt install -y nginx apache2 ufw fail2ban


Flag	Full form	What it does	Without it

-y	--yes	        Automatically answers "yes" to prompts	Apt will ask "Do you want to continue? [Y/n]" and wait


Command 3: sudo apt autoremove -y

Same as above

Command 4: sudo systemctl list-unit-files --type=service | grep enabled

Part	        What it does
systemctl	Command to control systemd
list-unit-files	Show all unit files (services, sockets, etc.)
--type=service	Flag: Show only service units
| grep enabled	Filter to show only lines with "enabled"


HYPOTHESIS

sudo apt update

Err:1 http://archive.ubuntu.com/ubuntu jammy InRelease
  Temporary failure resolving 'archive.ubuntu.com'

Observation: DNS resolution failed for Ubuntu repository.

Hypothesis: No internet connection or DNS problem.



Solution: Ping -c 4 8.8.8.8

Scenario 2: Installation Fails – Package Not Found

sudo apt install -y nginx

E: Unable to locate package nginx

Observation: Apt can't find the package.

Hypothesis: Forgot to run sudo apt update first, or repository list is wrong.


Scenario 4: Service Not Starting After Install

sudo apt install -y nginx
systemctl status nginx

● nginx.service - A high performance web server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled)
     Active: failed (Result: exit-code)

Observation: Service installed but failed to start.

Hypothesis: Port conflict (something else on port 80) or config error.

How to test:

# Check what's on port 80
ss -tulpen | grep :80

# Check logs
sudo journalctl -u nginx --since "1 min ago"

Quiz


1 Apt update checks available softwares or packages .Apt upgrade provides latest versions
2. For security
3. Apt autoremove
4. For periodic reviews and protection
5. Nginx and Apache

My response:

1 Apt update checks available softwares or packages .Apt upgrade provides latest versions
2. For security
3. Apt autoremove
4. For periodic reviews and protection
5. Nginx and Apache

AI:

Your Answers – Marked
Question 1: Difference between apt update and apt upgrade?

Your answer: "Apt update checks available softwares or packages. Apt upgrade provides latest versions"

✅ CORRECT! Simple and accurate:

    apt update = refreshes the package list (what's available)

    apt upgrade = actually installs newer versions

Question 2: Why pin or verify package sources?

Your answer: "For security"

✅ CORRECT! Verifying sources ensures you're not installing malicious software from untrusted places.
Question 3: What command removes unused dependencies?

Your answer: apt autoremove

✅ CORRECT! The full command is sudo apt autoremove
Question 4: Why apply security updates regularly?

Your answer: "For periodic reviews and protection"

✅ CORRECT! Security updates fix vulnerabilities that attackers could exploit.
Question 5: Name one package for web serving.

Your answer: "Nginx and Apache"

✅ CORRECT! Both are web servers. The question said "one" but you gave two – that's fine!

