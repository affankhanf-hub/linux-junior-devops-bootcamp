## 📚 Theory Summary

Package management is how Linux installs, updates, and removes software safely from trusted repositories.

| Concept | What It Means |
|---------|----------------|
| `sudo apt update` | Refreshes the list of available packages from repositories. Must run before installing anything. |
| `sudo apt install -y` | Installs packages automatically without asking for confirmation. The `-y` flag answers "yes" automatically. |
| Dependencies | Other packages that software needs to work. `apt` handles them automatically. |
| `sudo apt autoremove -y` | Removes unused dependencies that were installed but are no longer needed. Frees disk space. |
| Baseline hardening | Making your system more secure by patching, restricting SSH access, enforcing firewall defaults, and removing unnecessary services. |
| Enabled services | Every enabled service is a potential attack surface. Disable what you don't need. |
| UFW (Uncomplicated Firewall) | Should be set to default deny incoming, allow outgoing, and only open specific ports like 22 (SSH) and 80 (HTTP). |

---

## 🔧 Commands Used

| # | Command | Purpose |
|---|---------|---------|
| 1 | `sudo apt update` | Refresh package list from repositories |
| 2 | `sudo apt install -y nginx apache2 ufw fail2ban` | Install web servers, firewall, and intrusion prevention |
| 3 | `dpkg -l \| grep <package>` | Verify packages are installed |
| 4 | `systemctl status nginx --no-pager \| head -5` | Check if service is running |
| 5 | `sudo apt autoremove -y` | Remove unused dependencies |
| 6 | `sudo systemctl list-unit-files --type=service \| grep enabled` | List all services that start at boot |

---

## 📤 Output & Evidence

### Task 1: `sudo apt update`

**Command:**
```bash
sudo apt update
Output (abridged):

text
Hit:1 http://de.archive.ubuntu.com/ubuntu noble InRelease
Get:2 http://de.archive.ubuntu.com/ubuntu noble-updates InRelease [126 kB]
Get:3 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]
Fetched 14.7 MB in 11s (1,351 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
12 packages can be upgraded. Run 'apt list --upgradable' to see them.
What this proves: Package list refreshed successfully. 12 updates available.

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

nginx, apache2, ufw already installed

fail2ban and dependencies installed successfully

fail2ban service automatically enabled

Task 3: sudo apt autoremove -y
Command:

bash
sudo apt autoremove -y
Output:

text
The following packages will be REMOVED:
  libllvm19
After this operation, 129 MB disk space will be freed.
Removing libllvm19:amd64 (1:19.1.1-1ubuntu1~24.04.2) ...
What this proves: 129 MB of unused dependencies removed.

✅ Validation (Proof of Success)
Package Installation Verification
Package	Verification Command	Status
nginx	dpkg -l | grep nginx	✅ ii nginx 1.24.0-2ubuntu7.6
apache2	dpkg -l | grep apache2	✅ ii apache2 2.4.58-1ubuntu8.10
ufw	dpkg -l | grep ufw	✅ ii ufw 0.36.2-6
fail2ban	dpkg -l | grep fail2ban	✅ ii fail2ban 1.0.2-3ubuntu0.1
Note: ii = installed successfully

Service Status Verification
bash
systemctl status nginx --no-pager | head -5
text
● nginx.service - A high performance web server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled)
     Active: active (running) since Wed 2026-03-04 16:30:48 CET
✅ CONFIRMED: nginx is enabled and running

📋 Maintenance Checklist
What was installed	Why it was installed	Service enabled?	What should be disabled or reviewed
nginx	Web server to serve content	✅ Enabled	Keep - review security headers
apache2	Second web server for learning	❌ Disabled	Review - consider removing (port conflict risk)
ufw	Firewall to control network traffic	✅ Enabled	Keep - ensure "deny incoming" default
fail2ban	Blocks brute force attacks (SSH, web)	✅ Enabled	Keep - review jail configurations
cups (printing)	Print service (desktop default)	✅ Enabled	❌ Disable - not needed on server
bluetooth	Bluetooth stack	✅ Enabled	❌ Disable - VM has no Bluetooth
avahi-daemon	Network service discovery (mDNS)	✅ Enabled	❌ Disable - information leakage risk
ModemManager	Modem management	✅ Enabled	❌ Disable - not needed on VM
Commands to disable unnecessary services:

bash
sudo systemctl disable --now cups bluetooth avahi-daemon ModemManager
🔒 Security Relevance
Why Updates Matter
Action	Security Impact
sudo apt update	Ensures system knows about latest security patches
sudo apt upgrade	Fixes known CVEs (Common Vulnerabilities and Exposures)
12 upgradable packages found	Each may contain unpatched flaws - delay increases risk
Why Service Exposure Matters
Service	Enabled	Risk Level	Action
nginx	✅	Low (if configured correctly)	Keep with secure config
fail2ban	✅	Low (protective service)	Keep
ufw	✅	Low (blocks unwanted traffic)	Keep
cups	✅	Medium - print service has RCE history	Disable
bluetooth	✅	Medium - known vulnerabilities (BlueBorne)	Disable
avahi-daemon	✅	Low-Medium - information leakage	Disable
Key Principle Learned
Every enabled service is a potential attack surface.

The systemctl list-unit-files | grep enabled command showed 80+ services starting at boot. Most (printing, Bluetooth, network discovery, ModemManager) are unnecessary on a VM or server.

Security best practice: Disable everything, then enable only what you need.

🐛 Troubleshooting
No actual errors in main tasks
All commands ran successfully. Only warnings were SyntaxWarning messages during fail2ban installation (escape sequences in test files - no functional impact).

Hypothetical scenarios studied
Scenario	Observation	Hypothesis	Solution
Update fails	Temporary failure resolving 'archive.ubuntu.com'	DNS/network issue	ping -c 4 8.8.8.8
Package not found	E: Unable to locate package nginx	Forgot apt update	Run sudo apt update first
Service fails to start	Active: failed (Result: exit-code)	Port conflict or config error	ss -tulpen | grep :80 and journalctl -u nginx
📝 Lessons Learned
apt update before apt install - Package lists get stale without updating.

-y flag hides prompts - Good for automation, but learn without it first.

dpkg -l shows truth - ii means installed. Any other letters = problem.

Installed ≠ running - nginx was installed AND running. apache2 installed but NOT enabled. Check both.

autoremove finds garbage - Found 129 MB of unused dependencies (libllvm19). Regular cleanup saves space and reduces attack surface.

Many unnecessary services run by default - cups, bluetooth, avahi-daemon all running on VM. Disable for servers.

Both nginx + apache2 = port conflict - Both want port 80. Only one can run. Disable apache2.

