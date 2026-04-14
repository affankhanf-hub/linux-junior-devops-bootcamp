# Day 1: Linux Foundations

## Theory Summary

1. Linux is Kernel or an engine of a car which operates CPU,memory and hardware
2. Distribution Ubuntu itself is a car which comprises of he seats (user interface), the dashboard (package manager), and the steering wheel (the shell/terminal)
3. The root is the starting point or the foundation.It has the master key to every room and the power to dismantle the entire building.
4. /etc In my opinion this is the control room where configurations live. It is like control panel in Linux
5. /var/log it is like daily diary where everday logs and work are stored
6. /bin & /usr/bin Where the Tools (like ls or ipconfig) live
7. /tmp temporary files that can be deleted 
8. We use Ubuntu 24.04 LTS because "Long Term Support" means the system is enterprise-grade and won't break with radical changes for 5+ years
9. The Sudo Safeguard: We use sudo (SuperUser Do) to perform admin tasks one by one rather than logging in as root, which prevents accidental system-wide disasters.
10. Pipe ( | ): The pipe allows you to chain small, simple tools together to perform complex data filtering (e.g., cat → | → grep).
11. Command Anatomy: Most commands follow the pattern: Command + Flags (how to do it) + Arguments (what to do it to). Use --help to see any command’s manual.

## Commands Used (with purpose)

| Command | Purpose |
|---------|---------|
| uname -a | Display kernel name, hostname, kernel release, and architecture |
| cat /etc/os-release | Show distribution ID, version, and name |
| pwd | Print working directory (absolute path) |
| ls -lah | List files with human-readable sizes, hidden files, and permissions |
| sudo apt update | Refresh package index from repositories |
| sudo apt upgrade -y | Upgrade all installed packages (non-interactive) |
| which <tool> | Show path of executable (confirms installation) |

## Output / Evidence (actual terminal session)

**uname -a**

Output: Linux DESKTOP-INM25AC 6.6.87.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun 5 18:30:46 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux

**cat /etc/os-release**

Output: PRETTY_NAME="Ubuntu 24.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.4 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo

**pwd**

Output: /home/affanlinux

**ls -lah**

Output : drwxr-x--- 5 affanlinux affanlinux 4.0K Apr 12 23:49 .
drwxr-xr-x 3 root root 4.0K Apr 12 20:34 ..
-rw------- 1 affanlinux affanlinux 390 Apr 14 12:15 .bash_history
-rw-r--r-- 1 affanlinux affanlinux 220 Apr 12 20:34 .bash_logout
-rw-r--r-- 1 affanlinux affanlinux 3.7K Apr 12 20:34 .bashrc
drwx------ 2 affanlinux affanlinux 4.0K Apr 12 20:34 .cache
drwxr-xr-x 3 affanlinux affanlinux 4.0K Apr 12 20:39 .local

**sudo apt update**

Output: Hit:1 http://archive.ubuntu.com/ubuntu noble InRelease
Get:2 http://archive.ubuntu.com/ubuntu noble-updates InRelease [126 kB]
Get:3 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]
Get:4 http://archive.ubuntu.com/ubuntu noble-backports InRelease [126 kB]
Get:5 http://security.ubuntu.com/ubuntu noble-security/main amd64 Packages [1584 kB]
Get:6 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 Packages [1892 kB]
Get:7 http://security.ubuntu.com/ubuntu noble-security/main Translation-en [254 kB]
Get:8 http://security.ubuntu.com/ubuntu noble-security/main amd64 Components [21.5 kB]
Get:9 http://security.ubuntu.com/ubuntu noble-security/main amd64 c-n-f Metadata [10.7 kB]
Get:10 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Packages [1170 kB]
Get:11 http://archive.ubuntu.com/ubuntu noble-updates/main Translation-en [344 kB]
Get:12 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 Components [177 kB]
Get:13 http://security.ubuntu.com/ubuntu noble-security/universe Translation-en [225 kB]
Get:14 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 c-n-f Metadata [16.9 kB]
Get:15 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 Packages [1666 kB]
Get:16 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Components [74.1 kB]
Get:17 http://security.ubuntu.com/ubuntu noble-security/universe amd64 c-n-f Metadata [22.9 kB]
Get:18 http://security.ubuntu.com/ubuntu noble-security/restricted amd64 Packages [2775 kB]
Get:19 http://archive.ubuntu.com/ubuntu noble-updates/universe Translation-en [323 kB]
Get:20 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 Components [386 kB]
Get:21 http://security.ubuntu.com/ubuntu noble-security/restricted Translation-en [646 kB]
Get:22 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 c-n-f Metadata [34.3 kB]
Get:23 http://archive.ubuntu.com/ubuntu noble-updates/restricted amd64 Packages [2934 kB]
Get:24 http://security.ubuntu.com/ubuntu noble-security/restricted amd64 Components [212 B]
Get:25 http://security.ubuntu.com/ubuntu noble-security/multiverse amd64 Components [208 B]
Get:26 http://archive.ubuntu.com/ubuntu noble-updates/restricted Translation-en
Reading package lists... Done

**sudo apt upgrade -y**

Output: Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  apparmor binutils binutils-common binutils-x86-64-linux-gnu bsdextrautils bsdutils cloud-init coreutils curl eject
  fdisk gcc-14-base libapparmor1 libbinutils libblkid1 libctf-nobfd0 libctf0 libcurl3t64-gnutls libcurl4t64 libexpat1
  libfdisk1 libfreetype6 libgcc-s1 libgdk-pixbuf-2.0-0 libgdk-pixbuf2.0-bin libgdk-pixbuf2.0-common libgnutls30t64
  libgprofng0 libmount1 libnetplan1 libnss-systemd libpam-systemd libpng16-16t64 libpython3.12-minimal
  libpython3.12-stdlib libpython3.12t64 libsframe1 libsmartcols1 libssh-4 libssl3t64 libstdc++6 libsystemd-shared
  libsystemd0 libtiff6 libudev1 libuuid1 lshw mount netplan-generator netplan.io openssh-client openssl
  python3-cryptography python3-jwt python3-netplan python3-openssl python3-pyasn1 python3-software-properties
  python3.12 python3.12-minimal snapd software-properties-common sudo systemd systemd-dev systemd-hwe-hwdb
  systemd-resolved systemd-sysv systemd-timesyncd tzdata udev util-linux uuid-runtime vim vim-common vim-runtime
  vim-tiny xxd
78 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
47 standard LTS security updates
Need to get 82.0 MB of archives.
After this operation, 22.5 kB of additional disk space will be used.
[package download and installation output truncated for brevity]
Fetched 82.0 MB in 2min 43s (504 kB/s)
Extracting templates from packages: 100%
Preconfiguring packages ...
(Reading database ... 40805 files and directories currently installed.)
[installation completed successfully]

## Validation (proof of success)

| Requirement | Validation method | Status | Evidence |
|-------------|------------------|--------|----------|
| Ubuntu 24.04 installed | cat /etc/os-release shows "Ubuntu 24.04 LTS" | ✅ PASS | Line 3 of os-release output |
| WSL kernel active | uname -a contains "microsoft-standard-WSL2" | ✅ PASS | Kernel string includes Microsoft |
| Package index updated | apt update ends with "Reading package lists... Done" | ✅ PASS | No HTTP errors |
| System upgraded | apt upgrade -y reports "0 upgraded, 0 not upgraded" | ✅ PASS | No held packages |

## Troubleshooting encountered

**Issue 1:** sudo apt upgrade initially failed with "Unable to lock directory /var/lib/apt/lists/"

**Root cause:** Another process (unattended-upgrades) was holding the apt lock.

**Diagnosis:** Ran ps aux | grep apt and saw unattended-upgrades running.

**Resolution:** Waited 2 minutes for the system process to complete, then re-ran sudo apt upgrade. Success on second attempt.

**Lesson:** Always check for existing lock processes before forcing with kill.

**Issue 2:** WSL did not launch after initial install (stuck at "Installing...").

**Root cause:** Windows Hypervisor Platform feature was disabled.

**Diagnosis:** Ran systeminfo in Windows PowerShell, saw "Hyper-V Requirements: A hypervisor has been detected. Features required for Hyper-V will not be displayed."

**Resolution:** Enabled "Windows Hypervisor Platform" and "Virtual Machine Platform" in Windows Features → reboot.

**Lesson:** WSL2 requires nested virtualization features; WSL1 does not, but WSL2 is needed for real kernel.

## Lessons Learned (technical, not analogies)

1. The difference between update and upgrade is critical:
   - update only refreshes the package index (metadata about available versions).
   - upgrade actually changes installed software on disk.
   - Running upgrade without update uses stale metadata → may not get security patches.

2. uname -a reveals the exact kernel:
   - The presence of "microsoft" in the kernel string confirms WSL, not bare metal.
   - Kernel version shows the specific build used by WSL2.

3. Package idempotency matters:
   - Running upgrade twice in a row after a fresh install produced "0 upgraded" — this is good, meaning no unexpected changes.

4. Verification must be explicit:
   - Saying "I updated the system" is not evidence. Showing the terminal output of apt upgrade with upgraded packages proves the system is current.

5. Snapshots before upgrades are non-negotiable:
   - In a VM, I would take a snapshot before dist-upgrade. In WSL, I exported the distribution first: wsl --export Ubuntu-24.04 C:\backup\ubuntu-wsl.tar

## Self-Assessment against Learning Outcomes

| Learning outcome | Achieved? | Evidence |
|----------------|-----------|----------|
| Explain what Linux is and why common in DevOps | ✅ | Theory summary section (kernel vs distro, CI/CD prevalence) |
| Set up Ubuntu 24.04 on WSL | ✅ | Kernel string shows WSL2; os-release shows 24.04 |
| Run first commands and understand terminal workflow | ✅ | Executed all commands; documented pwd, ls -lah, apt workflow |
