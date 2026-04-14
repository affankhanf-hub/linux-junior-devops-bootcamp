Theory Summary 

1.Linux is Kernel or an engine of a car which operates CPU,memory and hardware

2.Distribution Ubuntu itself is a car which comprises of he seats (user interface), the dashboard (package manager), and the steering wheel (the shell/terminal)
3.The root is the starting point or the foundation.It has the master key to every room and the power to dismantle the entire building.
4./etc In my opinion this is the control room where configurations live. It is like control panel in Linux
5./var/log it is like daily diary where everday logs and work are stored
6./bin & /usr/bin Where the Tools (like ls or ipconfig) live
7./tmp temporary files that can be deleted
8.We use Ubuntu 24.04 LTS because "Long Term Support" means the system is enterprise-grade and won't break with radical changes for 5+ years 9.The Sudo Safeguard: We use sudo (SuperUser Do) to perform admin tasks one by one rather than logging in as root, which prevents accidental system-wide disasters.
9.Pipe ( | ): The pipe allows you to chain small, simple tools together to perform complex data filtering (e.g., cat → | → grep). 11.Command Anatomy: Most commands follow the pattern: Command + Flags (how to do it) + Arguments (what to do it to). Use --help to see any command’s manual.

Commands Used (with purpose)

Command         	Purpose
uname -a        	Display kernel name, hostname, kernel release, and architecture
cat /etc/os-release	Show distribution ID, version, and name
pwd             	Print working directory (absolute path)
ls -lah         	List files with human-readable sizes, hidden files, and permissions
sudo apt update	Refresh package index from repositories
sudo apt upgrade -y	Upgrade all installed packages (non-interactive)
which <tool>    	Show path of executable (confirms installation)


