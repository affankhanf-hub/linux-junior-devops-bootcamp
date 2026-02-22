1.The Root (/): In Linux, everything is a file and starts from the single root directory. There are no "C:" or "D:" drives like in Windows
2.Configuration (/etc): This is the "Settings" folder for the OS. If you need to change how a service behaves (like SSH or a Web Server), you find the config file here
3.Variable Data (/var/log): This is the most critical folder for troubleshooting. It stores the "black box" recording of everything the system does
4.Absolute Paths: These are "Full Addresses" starting from / (e.g., /home/user/file.txt). They work regardless of where you are in the terminal
5.Relative Paths: These are "Directions" based on your current location (e.g., ../scripts/test.sh). They are faster but depend on your pwd
6.Redirection (>): This "writes" output to a file. It is dangerous because it wipes the file clean (overwrites) before putting the new data in
7.Append (>>): This "adds" output to the bottom of a file. It is the safe way to keep a running log of your command results
8.Piping (|): The "Chain." It connects the output of one tool to the input of another, allowing you to filter huge amounts of data (like cat into grep)
9.Service Ports: Networking services live on specific "Ports." SSH uses Port 22 for secure remote access, and HTTP uses Port 80 for web traffic
10.Memory Strategy: Engineers don't memorize flags; they memorize "Patterns." Use man <command> to read the manual or <command> --help for a quick flag reminder

Lab results

mkdir -p ~/course/day02/{notes,logs,scripts}
Created notes, logs, and scripts directories

cd ~/linux-junior-devops-bootcamp/days/day02
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/days/day02$ ls
day02-cli-files-lab.md

find ~/course -type f
/home/affan-khan/course/day02/notes/lab_notes.txt


grep -i "error" /var/log/syslog | tail -n 20

026-02-22T00:29:01.255390+01:00 affan-khan-VirtualBox org.gnome.Nautilus[13792]: MESA: error: ZINK: failed to choose pdev
2026-02-22T00:34:25.991427+01:00 affan-khan-VirtualBox tracker-miner-fs-3[14184]: (tracker-extract-3:14184): GLib-GIO-WARNING **: 00:34:25.988: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)
2026-02-22T00:34:36.572228+01:00 affan-khan-VirtualBox org.gnome.Nautilus[14214]: MESA: error: ZINK: failed to choose pdev
2026-02-22T00:39:21.909429+01:00 affan-khan-VirtualBox tracker-miner-fs-3[14518]: (tracker-extract-3:14518): GLib-GIO-WARNING **: 00:39:21.906: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)
2026-02-22T00:40:05.825101+01:00 affan-khan-VirtualBox org.gnome.Nautilus[14683]: MESA: error: ZINK: failed to choose pdev
2026-02-22T01:23:19.994038+01:00 affan-khan-VirtualBox gnome-shell[2400]: libinput error: event2  - AT Translated Set 2 keyboard: client bug: event processing lagging behind by 72ms, your system is too slow
2026-02-22T02:02:26.014059+01:00 affan-khan-VirtualBox tracker-miner-fs-3[17129]: (tracker-extract-3:17129): GLib-GIO-WARNING **: 02:02:26.011: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)


tar -czf backup.tar.gz ~/course/day02
tar: Removing leading `/' from member names


affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/days/day02$ tar -tvf backup.tar.gz
rwxrwxr-x affan-khan/affan-khan 0 2026-02-22 02:02 home/affan-khan/course/day02/
drwxrwxr-x affan-khan/affan-khan 0 2026-02-22 02:02 home/affan-khan/course/day02/scripts/
drwxrwxr-x affan-khan/affan-khan 0 2026-02-22 02:02 home/affan-khan/course/day02/logs/
drwxrwxr-x affan-khan/affan-khan 0 2026-02-22 02:02 home/affan-khan/course/day02/notes/
-rw-rw-r-- affan-khan/affan-khan 0 2026-02-22 02:02 home/affan-khan/course/day02/notes/lab_notes.txt

Flags

Command	Flag	What it does (The "Why")

mkdir	-p	Parents: Creates the full path even if the middle folders don't exist.
-v	Verbose: Tells you exactly which folders it created in the terminal.

find	-type	Type: Filters the search (e.g., f for file, d for directory).
-name	Name: Searches for a specific filename or pattern (like *.txt).

grep	-i	Ignore Case: Finds "ERROR", "error", or "Error" all at once.
-v	Invert: Shows every line except the one containing the word you type.
tar	-c	Create: Tells the tool you want to build a new archive file.
-z	Gzip: Compresses the file so it takes up less disk space.
	
tail	-n	Number: Specifies exactly how many lines from the end to show.
	

One-Line Meanings
- **mkdir -p**: Standardized the project structure in one go.
- **find -type f**: Confirmed the automation of file creation.
- **grep | tail**: Conducted a targeted investigation of system health.
- **tar -czf**: Created a deployment-ready backup of the work.


Error-Driven Learning (Mistakes & Fixes Log)

This is where you document the "File not found" and "Tar warning" errors we just solved. This is what senior engineers look for in your reports.

   Mistake: Running find ~/course and getting No such file or directory.

   Fix: Realized the directory hadn't been created yet. Ran mkdir -p first.

   Mistake: Confusion over tar: Removing leading '/' from member names.

   Fix: Learned this is a safety feature to make paths relative for easier extraction.

   Mistake: Trying to run commands while nano was already occupying the terminal.

   Fix: Used a second terminal

Observation: I feel my system is slow
Hypothesis: If the system is slow than log will show errors about hardware processing or slow events
Command:`grep -i "error" /var/log/syslog | tail -n 10`
Evidence: i "error" /var/log/syslog | tail -n 10
2026-02-22T00:29:01.255390+01:00 affan-khan-VirtualBox org.gnome.Nautilus[13792]: MESA: error: ZINK: failed to choose pdev
2026-02-22T00:34:25.991427+01:00 affan-khan-VirtualBox tracker-miner-fs-3[14184]: (tracker-extract-3:14184): GLib-GIO-WARNING **: 00:34:25.988: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)
2026-02-22T00:34:36.572228+01:00 affan-khan-VirtualBox org.gnome.Nautilus[14214]: MESA: error: ZINK: failed to choose pdev
2026-02-22T00:39:21.909429+01:00 affan-khan-VirtualBox tracker-miner-fs-3[14518]: (tracker-extract-3:14518): GLib-GIO-WARNING **: 00:39:21.906: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)
2026-02-22T00:40:05.825101+01:00 affan-khan-VirtualBox org.gnome.Nautilus[14683]: MESA: error: ZINK: failed to choose pdev
2026-02-22T01:23:19.994038+01:00 affan-khan-VirtualBox gnome-shell[2400]: libinput error: event2  - AT Translated Set 2 keyboard: client bug: event processing lagging behind by 72ms, your system is too slow
2026-02-22T02:02:26.014059+01:00 affan-khan-VirtualBox tracker-miner-fs-3[17129]: (tracker-extract-3:17129): GLib-GIO-WARNING **: 02:02:26.011: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)

Result: Hypothesis is positive . System is slow

The Official Day 2 Quiz

    Difference between absolute and relative path?

    What does | do in shell?

    Command to search text in file

    Purpose of tar?

    Why use head and tail?

My answers on Gemini

1. Absolute starts from / and relative from the current path 
2. Pipe it connects different tools 
3. Grep
4 Tar it compresses file to minimize usage of space
5 Head is used to check initial logs and tail is used to check current logs or work

Gemini response

Spot on. You’ve got the core logic down. Here is a tiny bit of "Senior Dev" nuance to add to your mental notes:

 On #4 (Tar): Technically, tar just "bundles" files together. The -z flag you used is what actually does the compression (minimizing space).

  On #5 (Head/Tail): Correct. head is also great for checking file headers/metadata, while tail -f is the industry standard for "watching" a crash happen in real-time.
