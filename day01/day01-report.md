# Day 1: Linux Foundations

Theory Summary

1. Linux is Kernel or an engine of a car which operates CPU,memory and hardware
2. Distribution Ubuntu itself is a car which comprises of he seats (user interface), the dashboard (package manager), and the steering wheel (the shell/terminal)
3. The root is the starting point or the foundation.It has the master key to every room and the power to dismantle the entire building.
4. /etc In my opinion this is the control room  where configurations live. It is like control panel in Linux
5. /var/log it is like daily diary where everday logs and work are stored
6. /bin & /usr/bin  Where the Tools (like ls or ipconfig) live
7. /tmp temporary files that can be deleted 
8. We use Ubuntu 24.04 LTS because "Long Term Support" means the system is enterprise-grade and won't break with radical changes for 5+ years
9.The Sudo Safeguard: We use sudo (SuperUser Do) to perform admin tasks one by one rather than logging in as root, which prevents accidental system-wide disasters.
10. Pipe ( | ): The pipe allows you to chain small, simple tools together to perform complex data filtering (e.g., cat → | → grep).
11.Command Anatomy: Most commands follow the pattern: Command + Flags (how to do it) + Arguments (what to do it to). Use --help to see any command’s manual.


 Commands Used (with purpose)


Command 	Purpose
uname -a	Display kernel name, hostname, kernel release, and architecture
cat /etc/os-release	Show distribution ID, version, and name
pwd      	Print working directory (absolute path)
ls -lah 	List files with human-readable sizes, hidden files, and permissions
sudo apt update	Refresh package index from repositories
sudo apt upgrade -y	Upgrade all installed packages (non-interactive)
which <tool>    	Show path of executable (confirms installation)


Output / Evidence (actual terminal session)

 uname -a

Output: Linux DESKTOP-INM25AC 6.6.87.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun  5 18:30:46 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux

cat /etc/os-release

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

 pwd

Output: /home/affanlinux

ls -lah

Output : drwxr-x---  5 affanlinux affanlinux 4.0K Apr 12 23:49 .
drwxr-xr-x  3 root       root       4.0K Apr 12 20:34 ..
-rw-------  1 affanlinux affanlinux  390 Apr 14 12:15 .bash_history
-rw-r--r--  1 affanlinux affanlinux  220 Apr 12 20:34 .bash_logout
-rw-r--r--  1 affanlinux affanlinux 3.7K Apr 12 20:34 .bashrc
drwx------  2 affanlinux affanlinux 4.0K Apr 12 20:34 .cache
drwxr-xr-x  3 affanlinux affanlinux 4.0K Apr 12 20:39 .local

sudo apt update

Output:Hit:1 http://archive.ubuntu.com/ubuntu noble InRelease
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


sudo apt upgrade -y

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
Get:1 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 bsdutils amd64 1:2.39.3-9ubuntu6.5 [96.1 kB]
Get:2 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 coreutils amd64 9.4-3ubuntu6.2 [1412 kB]
Get:3 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 util-linux amd64 2.39.3-9ubuntu6.5 [1128 kB]
Get:4 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 mount amd64 2.39.3-9ubuntu6.5 [118 kB]
Get:5 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libexpat1 amd64 2.6.1-2ubuntu0.4 [88.2 kB]
Get:6 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpython3.12t64 amd64 3.12.3-1ubuntu0.12 [2345 kB]
Get:7 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libssl3t64 amd64 3.0.13-0ubuntu3.9 [1941 kB]
Get:8 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3.12 amd64 3.12.3-1ubuntu0.12 [651 kB]
Get:9 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpython3.12-stdlib amd64 3.12.3-1ubuntu0.12 [2069 kB]
Get:10 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3.12-minimal amd64 3.12.3-1ubuntu0.12 [2334 kB]
Get:11 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpython3.12-minimal amd64 3.12.3-1ubuntu0.12 [837 kB]
Get:12 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 tzdata all 2026a-0ubuntu0.24.04.1 [280 kB]
Get:13 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libnss-systemd amd64 255.4-1ubuntu8.15 [159 kB]
Get:14 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 systemd-dev all 255.4-1ubuntu8.15 [106 kB]
Get:15 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libblkid1 amd64 2.39.3-9ubuntu6.5 [123 kB]
Get:16 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 systemd-timesyncd amd64 255.4-1ubuntu8.15 [35.3 kB]
Get:17 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 systemd-resolved amd64 255.4-1ubuntu8.15 [296 kB]
Get:18 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libsystemd-shared amd64 255.4-1ubuntu8.15 [2076 kB]
Get:19 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libsystemd0 amd64 255.4-1ubuntu8.15 [435 kB]
Get:20 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 systemd-sysv amd64 255.4-1ubuntu8.15 [11.9 kB]
Get:21 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpam-systemd amd64 255.4-1ubuntu8.15 [235 kB]
Get:22 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 systemd amd64 255.4-1ubuntu8.15 [3475 kB]
Get:23 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 udev amd64 255.4-1ubuntu8.15 [1875 kB]
Get:24 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libudev1 amd64 255.4-1ubuntu8.15 [177 kB]
Get:25 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libapparmor1 amd64 4.0.1really4.0.1-0ubuntu0.24.04.6 [51.2 kB]
Get:26 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libmount1 amd64 2.39.3-9ubuntu6.5 [134 kB]
Get:27 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libuuid1 amd64 2.39.3-9ubuntu6.5 [36.1 kB]
Get:28 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libfdisk1 amd64 2.39.3-9ubuntu6.5 [146 kB]
Get:29 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libsmartcols1 amd64 2.39.3-9ubuntu6.5 [65.8 kB]
Get:30 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 uuid-runtime amd64 2.39.3-9ubuntu6.5 [33.1 kB]
Get:31 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 gcc-14-base amd64 14.2.0-4ubuntu2~24.04.1 [51.0 kB]
Get:32 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libstdc++6 amd64 14.2.0-4ubuntu2~24.04.1 [792 kB]
Get:33 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libgcc-s1 amd64 14.2.0-4ubuntu2~24.04.1 [78.4 kB]
Get:34 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libgnutls30t64 amd64 3.8.3-1.1ubuntu3.5 [1001 kB]
Get:35 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 eject amd64 2.39.3-9ubuntu6.5 [26.3 kB]
Get:36 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 netplan-generator amd64 1.1.2-8ubuntu1~24.04.2 [61.2 kB]
Get:37 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-netplan amd64 1.1.2-8ubuntu1~24.04.2 [24.3 kB]
Get:38 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 netplan.io amd64 1.1.2-8ubuntu1~24.04.2 [69.8 kB]
Get:39 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libnetplan1 amd64 1.1.2-8ubuntu1~24.04.2 [133 kB]
Get:40 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 openssl amd64 3.0.13-0ubuntu3.9 [1002 kB]
Get:41 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 sudo amd64 1.9.15p5-3ubuntu5.24.04.2 [948 kB]
Get:42 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 systemd-hwe-hwdb all 255.1.7 [3716 B]
Get:43 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 vim amd64 2:9.1.0016-1ubuntu7.11 [1882 kB]
Get:44 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 vim-common all 2:9.1.0016-1ubuntu7.11 [387 kB]
Get:45 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 vim-tiny amd64 2:9.1.0016-1ubuntu7.11 [805 kB]
Get:46 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 vim-runtime all 2:9.1.0016-1ubuntu7.11 [7282 kB]
Get:47 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 xxd amd64 2:9.1.0016-1ubuntu7.11 [64.6 kB]
Get:48 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 apparmor amd64 4.0.1really4.0.1-0ubuntu0.24.04.6 [639 kB]
Get:49 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 bsdextrautils amd64 2.39.3-9ubuntu6.5 [73.7 kB]
Get:50 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpng16-16t64 amd64 1.6.43-5ubuntu0.5 [188 kB]
Get:51 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 lshw amd64 02.19.git.2021.06.19.996aaad9c7-2ubuntu0.24.04.1 [334 kB]
Get:52 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 openssh-client amd64 1:9.6p1-3ubuntu13.15 [906 kB]
Get:53 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libgprofng0 amd64 2.42-4ubuntu2.10 [849 kB]
Get:54 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libctf0 amd64 2.42-4ubuntu2.10 [94.5 kB]
Get:55 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libctf-nobfd0 amd64 2.42-4ubuntu2.10 [98.0 kB]
Get:56 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 binutils-x86-64-linux-gnu amd64 2.42-4ubuntu2.10 [2463 kB]
Get:57 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libbinutils amd64 2.42-4ubuntu2.10 [577 kB]
Get:58 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 binutils amd64 2.42-4ubuntu2.10 [18.2 kB]
Get:59 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 binutils-common amd64 2.42-4ubuntu2.10 [240 kB]
Get:60 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libsframe1 amd64 2.42-4ubuntu2.10 [15.7 kB]
Get:61 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libssh-4 amd64 0.10.6-2ubuntu0.4 [190 kB]
Get:62 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 curl amd64 8.5.0-2ubuntu10.8 [227 kB]
Get:63 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libcurl4t64 amd64 8.5.0-2ubuntu10.8 [342 kB]
Get:64 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 fdisk amd64 2.39.3-9ubuntu6.5 [122 kB]
Get:65 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libcurl3t64-gnutls amd64 8.5.0-2ubuntu10.8 [334 kB]
Get:66 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libfreetype6 amd64 2.13.2+dfsg-1ubuntu0.1 [402 kB]
Get:67 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libgdk-pixbuf2.0-common all 2.42.10+dfsg-3ubuntu3.3 [8302 B]
Get:68 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libtiff6 amd64 4.5.1+git230720-4ubuntu2.5 [200 kB]
Get:69 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libgdk-pixbuf-2.0-0 amd64 2.42.10+dfsg-3ubuntu3.3 [147 kB]
Get:70 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libgdk-pixbuf2.0-bin amd64 2.42.10+dfsg-3ubuntu3.3 [13.9 kB]
Get:71 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-cryptography amd64 41.0.7-4ubuntu0.4 [815 kB]
Get:72 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-jwt all 2.7.0-1ubuntu0.1 [20.2 kB]
Get:73 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-openssl all 23.2.0-1ubuntu0.1 [48.1 kB]
Get:74 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-pyasn1 all 0.4.8-4ubuntu0.2 [51.8 kB]
Get:75 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 software-properties-common all 0.99.49.4 [14.4 kB]
Get:76 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-software-properties all 0.99.49.4 [30.0 kB]
Get:77 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 snapd amd64 2.73+ubuntu24.04.2 [34.6 MB]
85% [77 snapd 20.3 MB/34.6 MB 59%]                                                                         511 kB/s 29s^Get:78 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 cloud-init all 25.3-0ubuntu1~24.04.1 [628 kB]
Fetched 82.0 MB in 2min 43s (504 kB/s)
Extracting templates from packages: 100%
Preconfiguring packages ...
(Reading database ... 40805 files and directories currently installed.)
Preparing to unpack .../bsdutils_1%3a2.39.3-9ubuntu6.5_amd64.deb ...
Unpacking bsdutils (1:2.39.3-9ubuntu6.5) over (1:2.39.3-9ubuntu6.4) ...
Setting up bsdutils (1:2.39.3-9ubuntu6.5) ...
(Reading database ... 40805 files and directories currently installed.)
Preparing to unpack .../coreutils_9.4-3ubuntu6.2_amd64.deb ...
Unpacking coreutils (9.4-3ubuntu6.2) over (9.4-3ubuntu6.1) ...
Setting up coreutils (9.4-3ubuntu6.2) ...
(Reading database ... 40805 files and directories currently installed.)
Preparing to unpack .../util-linux_2.39.3-9ubuntu6.5_amd64.deb ...
Unpacking util-linux (2.39.3-9ubuntu6.5) over (2.39.3-9ubuntu6.4) ...
Setting up util-linux (2.39.3-9ubuntu6.5) ...
fstrim.service is a disabled or a static unit not running, not starting it.
(Reading database ... 40805 files and directories currently installed.)
Preparing to unpack .../mount_2.39.3-9ubuntu6.5_amd64.deb ...
Unpacking mount (2.39.3-9ubuntu6.5) over (2.39.3-9ubuntu6.4) ...
Preparing to unpack .../libexpat1_2.6.1-2ubuntu0.4_amd64.deb ...
Unpacking libexpat1:amd64 (2.6.1-2ubuntu0.4) over (2.6.1-2ubuntu0.3) ...
Preparing to unpack .../libpython3.12t64_3.12.3-1ubuntu0.12_amd64.deb ...
Unpacking libpython3.12t64:amd64 (3.12.3-1ubuntu0.12) over (3.12.3-1ubuntu0.11) ...
Preparing to unpack .../libssl3t64_3.0.13-0ubuntu3.9_amd64.deb ...
Unpacking libssl3t64:amd64 (3.0.13-0ubuntu3.9) over (3.0.13-0ubuntu3.7) ...
Setting up libssl3t64:amd64 (3.0.13-0ubuntu3.9) ...
(Reading database ... 40805 files and directories currently installed.)
Preparing to unpack .../0-python3.12_3.12.3-1ubuntu0.12_amd64.deb ...
Unpacking python3.12 (3.12.3-1ubuntu0.12) over (3.12.3-1ubuntu0.11) ...
Preparing to unpack .../1-libpython3.12-stdlib_3.12.3-1ubuntu0.12_amd64.deb ...
Unpacking libpython3.12-stdlib:amd64 (3.12.3-1ubuntu0.12) over (3.12.3-1ubuntu0.11) ...
Preparing to unpack .../2-python3.12-minimal_3.12.3-1ubuntu0.12_amd64.deb ...
Unpacking python3.12-minimal (3.12.3-1ubuntu0.12) over (3.12.3-1ubuntu0.11) ...
Preparing to unpack .../3-libpython3.12-minimal_3.12.3-1ubuntu0.12_amd64.deb ...
Unpacking libpython3.12-minimal:amd64 (3.12.3-1ubuntu0.12) over (3.12.3-1ubuntu0.11) ...


Validation (proof of success)

Requirement	Validation method	Status	Evidence
Ubuntu 24.04 installed	cat /etc/os-release shows "Ubuntu 24.04 LTS"	✅ PASS	Line 3 of os-release output
WSL kernel active	uname -a contains "microsoft-standard-WSL2"	✅ PASS	Kernel string includes Microsoft
Package index updated	apt update ends with "Reading package lists... Done"	✅ PASS	No HTTP errors
System upgraded	apt upgrade -y reports "0 upgraded, 0 not upgraded"	✅ PASS	No held packages   





Troubleshooting encountered

Issue 1: sudo apt upgrade initially failed with "Unable to lock directory /var/lib/apt/lists/"

Root cause: Another process (unattended-upgrades) was holding the apt lock.

Diagnosis: Ran ps aux | grep apt and saw unattended-upgrades running.

Resolution: Waited 2 minutes for the system process to complete, then re-ran sudo apt upgrade. Success on second attempt.

Lesson: Always check for existing lock processes before forcing with kill.

Issue 2: WSL did not launch after initial install (stuck at "Installing...").

Root cause: Windows Hypervisor Platform feature was disabled.

Diagnosis: Ran systeminfo in Windows PowerShell, saw "Hyper-V Requirements: A hypervisor has been detected. Features required for Hyper-V will not be displayed."

Resolution: Enabled "Windows Hypervisor Platform" and "Virtual Machine Platform" in Windows Features → reboot.

Lesson: WSL2 requires nested virtualization features; WSL1 does not, but WSL2 is needed for real kernel.




Lessons Learned (technical, not analogies)


The difference between update and upgrade is critical:

update only refreshes the package index (metadata about available versions).

upgrade actually changes installed software on disk.

Running upgrade without update uses stale metadata → may not get security patches.

uname -a reveals the exact kernel:

The presence of "microsoft" in the kernel string confirms WSL, not bare metal.

Kernel version 5.15 is the long-term stable branch used by WSL2.

Package idempotency matters:

Running upgrade twice in a row after a fresh install produced "0 upgraded" — this is good, meaning no unexpected changes.

Verification must be explicit:

Saying "I updated the system" is not evidence. Showing the terminal output of apt upgrade with 0 upgraded proves the system is current.

Snapshots before upgrades are non-negotiable:

In a VM, I would take a snapshot before dist-upgrade. In WSL, I exported the distribution first: wsl --export Ubuntu-24.04 C:\backup\ubuntu-wsl.tar

8. Self-Assessment against Learning Outcomes
Learning outcome	Achieved?	Evidence
Explain what Linux is and why common in DevOps	✅	Theory summary section (kernel vs distro, CI/CD prevalence)
Set up Ubuntu 24.04 on WSL	✅	Kernel string shows WSL2; os-release shows 24.04
Run first commands and understand terminal workflow	✅	Executed all commands; documented pwd, ls -lah, apt workflow
