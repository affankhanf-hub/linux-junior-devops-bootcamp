Theory Summary (What I Learned)
Package management is how Linux installs, updates, and removes software safely from trusted repositories. Here are the key concepts I learned:

Concept	What It Means
sudo apt update	Refreshes the list of available packages from repositories. Must run before installing anything.
sudo apt install -y	Installs packages automatically without asking for confirmation. The -y flag answers "yes" for me.
Dependencies	Other packages that software needs to work. apt handles them automatically.
sudo apt autoremove -y	Removes unused dependencies that were installed but are no longer needed. Frees up disk space.
Baseline hardening	Making your system more secure by patching, restricting SSH access, enforcing firewall defaults, and removing unnecessary services.
Enabled services	Every enabled service is a potential attack surface. Disable what you don't need.
UFW (Uncomplicated Firewall)	Should be set to default deny incoming, allow outgoing, and only open specific ports like 22 (SSH) and 80 (HTTP).
Commands Used
#	Command	Purpose
1	sudo apt update	Refresh package list from repositories
2	sudo apt install -y nginx apache2 ufw fail2ban	Install web servers, firewall, and intrusion prevention
3	dpkg -l | grep <package>	Verify packages are installed
4	systemctl status nginx --no-pager | head -5	Check if service is running
5	sudo apt autoremove -y	Remove unused dependencies
6	sudo systemctl list-unit-files --type=service | grep enabled	List all services that start at boot
Output & Evidence
Task 1: sudo apt update
Command:

bash
sudo apt update
Output:

text
Hit:1 http://de.archive.ubuntu.com/ubuntu noble InRelease
Get:2 http://de.archive.ubuntu.com/ubuntu noble-updates InRelease [126 kB]
Get:3 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]
... (additional package downloads) ...
Fetched 14.7 MB in 11s (1,351 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
12 packages can be upgraded. Run 'apt list --upgradable' to see them.
What this proves: The package list was successfully refreshed. I can see that 12 packages have updates available.

Task 2: sudo apt install -y nginx apache2 ufw fail2ban
Command:

bash
sudo apt install -y nginx apache2 ufw fail2ban
Output (key parts):

text
nginx is already the newest version (1.24.0-2ubuntu7.6).
apache2 is already the newest version (2.4.58-1ubuntu8.10).
ufw is already the newest version (0.36.2-6).
The following NEW packages will be installed:
  fail2ban python3-pyasyncore python3-pyinotify python3-setuptools whois
Created symlink /etc/systemd/system/multi-user.target.wants/fail2ban.service → /usr/lib/systemd/system/fail2ban.service.
What this proves:

nginx, apache2, and ufw were already installed

fail2ban and its dependencies were successfully installed as new packages

The fail2ban service was automatically enabled (symlink created)

Validation (Proof of Success)
Validation 1: Check nginx is installed
bash
dpkg -l | grep nginx
text
ii  nginx                                         1.24.0-2ubuntu7.6                        amd64        small, powerful, scalable web/proxy server
ii  nginx-common                                  1.24.0-2ubuntu7.6                        all          small, powerful, scalable web/proxy server - common files
✅ CONFIRMED: nginx is installed (the ii means "installed")

Validation 2: Check apache2 is installed
bash
dpkg -l | grep apache2
text
ii  apache2                                       2.4.58-1ubuntu8.10                       amd64        Apache HTTP Server
ii  apache2-bin                                   2.4.58-1ubuntu8.10                       amd64        Apache HTTP Server (modules and other binary files)
ii  apache2-data                                  2.4.58-1ubuntu8.10                       all          Apache HTTP Server (common files)
ii  apache2-utils                                 2.4.58-1ubuntu8.10                       amd64        Apache HTTP Server (utility programs for web servers)
✅ CONFIRMED: apache2 is installed

Validation 3: Check ufw is installed
bash
dpkg -l | grep ufw
text
ii  ufw                                           0.36.2-6                                 all          program for managing a Netfilter firewall
✅ CONFIRMED: ufw is installed

Validation 4: Check fail2ban is installed
bash
dpkg -l | grep fail2ban
text
ii  fail2ban                                      1.0.2-3ubuntu0.1                         all          ban hosts that cause multiple authentication errors
✅ CONFIRMED: fail2ban is installed

Validation 5: Check nginx is running
bash
systemctl status nginx --no-pager | head -5
text
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-03-04 16:30:48 CET; 10h ago
       Docs: man:nginx(8)
   Main PID: 1259 (nginx)
✅ CONFIRMED: nginx is enabled (starts at boot) and active (running)

Validation 6: Check autoremove worked
bash
sudo apt autoremove -y
text
The following packages will be REMOVED:
  libllvm19
After this operation, 129 MB disk space will be freed.
Removing libllvm19:amd64 (1:19.1.1-1ubuntu1~24.04.2) ...
✅ CONFIRMED: libllvm19 (129 MB) was removed. This was a dependency no longer needed by any installed package.

Maintenance Checklist
What was installed	Why it was installed	Service enabled?	What should be disabled or reviewed
nginx	Web server to serve web content	✅ Enabled	Keep - but review configuration for security headers
apache2	Web server (second one for learning)	❌ Disabled	Review - having both nginx and apache2 creates port conflict risk. Consider removing: sudo apt remove apache2
ufw	Firewall to control network traffic	✅ Enabled	Keep - but ensure default policy is "deny incoming"
fail2ban	Blocks brute force attacks (SSH, web logins)	✅ Enabled	Keep - review jails configuration
cups (printing)	Print service (auto-installed with desktop)	✅ Enabled	❌ Should disable - servers don't need printing
bluetooth	Bluetooth stack	✅ Enabled	❌ Should disable - VMs have no Bluetooth hardware
avahi-daemon	Network service discovery (mDNS)	✅ Enabled	❌ Should disable - information leakage risk on servers
ModemManager	Modem management	✅ Enabled	❌ Should disable - not needed on VM
Commands to disable unnecessary services:

bash
sudo systemctl disable --now cups bluetooth avahi-daemon ModemManager
Security Relevance of Updates & Service Exposure
Why Updates Matter for Security
Action	Security Impact
Running sudo apt update regularly	Ensures my system knows about the latest security patches. Without this, I would install outdated (vulnerable) versions.
Actually applying upgrades (sudo apt upgrade)	Fixes known CVEs (Common Vulnerabilities and Exposures). Attackers share exploit code for unpatched vulnerabilities.
The 12 upgradable packages in my output	Each of these 12 packages may contain unpatched security flaws. Delaying upgrades = leaving doors open.
Why Service Exposure Matters
Service	Enabled?	Risk if Exposed	Action Needed
nginx	✅ Enabled	Low risk if configured correctly. But every web server is a potential entry point for web application attacks.	Keep, but secure configuration required
fail2ban	✅ Enabled	Low risk - it's a protector, not an attack surface	Keep
ufw	✅ Enabled	Low risk - firewall rules block unwanted traffic	Keep
cups	✅ Enabled	MEDIUM RISK - Print service has had remote code execution vulnerabilities in the past. A server does not need printing.	Disable immediately
bluetooth	✅ Enabled	MEDIUM RISK - Bluetooth has known vulnerabilities (BlueBorne, etc.). Servers have no Bluetooth hardware.	Disable immediately
avahi-daemon	✅ Enabled	LOW-MEDIUM RISK - Network discovery service can leak information about my system to the local network.	Disable for servers
The Key Principle I Learned
Every enabled service is a potential attack surface.

Just because a service is installed doesn't mean it needs to be running. My systemctl list-unit-files | grep enabled command showed me over 80 services that start automatically at boot. Most of them (printing, Bluetooth, network discovery, speech dispatcher, ModemManager) are completely unnecessary on a virtual machine or server.

Security best practice: Disable everything, then enable only what you need.

Troubleshooting (What Went Wrong & How I Fixed It)
Issue: No actual errors occurred in my main tasks
Observation: All commands ran successfully on the first attempt. The only warning was SyntaxWarning messages during fail2ban installation (related to escape sequences in test files - these do not affect functionality).

What I learned: Even when things work, I should practice troubleshooting scenarios so I'm prepared when real failures happen.

Hypothetical Scenarios I Studied:
Scenario	Observation	Hypothesis	Solution
Update fails	Temporary failure resolving 'archive.ubuntu.com'	DNS problem or no internet	ping -c 4 8.8.8.8 to test connectivity
Package not found	E: Unable to locate package nginx	Forgot apt update or wrong repo	Run sudo apt update first
Service fails to start	Active: failed (Result: exit-code)	Port conflict or config error	Check ss -tulpen | grep :80 and journalctl -u nginx
Lessons Learned
apt update must come before apt install - The package list gets stale. Without updating, apt might not find the package or might install an old version.

The -y flag saves time but hides prompts - Good for scripts and automation, but when learning, it's better to run without -y to see what apt is asking.

dpkg -l is my proof of installation - The ii at the beginning of each line means "installed". Any other letters indicate problems.

Installed does NOT mean running - nginx was installed AND running. apache2 was installed but NOT enabled. I need to check both.

autoremove found 129 MB of garbage - The libllvm19 package was left over from something else. Regular cleanup saves disk space and reduces attack surface.

Many services run by default that I don't need - Looking at the grep enabled output, I see cups (printing), bluetooth, avahi-daemon all running on my VM. These should be disabled for a server.

Both nginx and apache2 installed = port conflict risk - Both want port 80. Only one can run at a time. I should disable apache2 since nginx is already running.
