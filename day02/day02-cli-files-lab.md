# Day 2: CLI and Filesystem

## Theory Summary

1. **The Root (/):** In Linux, everything is a file and starts from the single root directory. There are no "C:" or "D:" drives like in Windows.

2. **Configuration (/etc):** This is the "Settings" folder for the OS. If you need to change how a service behaves (like SSH or a Web Server), you find the config file here.

3. **Variable Data (/var/log):** This is the most critical folder for troubleshooting. It stores the "black box" recording of everything the system does.

4. **Absolute Paths:** These are "Full Addresses" starting from / (e.g., /home/user/file.txt). They work regardless of where you are in the terminal.

5. **Relative Paths:** These are "Directions" based on your current location (e.g., ../scripts/test.sh). They are faster but depend on your pwd.

6. **Redirection (>):** This "writes" output to a file. It is dangerous because it wipes the file clean (overwrites) before putting the new data in.

7. **Append (>>):** This "adds" output to the bottom of a file. It is the safe way to keep a running log of your command results.

8. **Piping (|):** The "Chain." It connects the output of one tool to the input of another, allowing you to filter huge amounts of data (like cat into grep).

9. **Service Ports:** Networking services live on specific "Ports." SSH uses Port 22 for secure remote access, and HTTP uses Port 80 for web traffic.

10. **Memory Strategy:** Engineers don't memorize flags; they memorize "Patterns." Use man <command> to read the manual or <command> --help for a quick flag reminder.

## Commands Used

| Command | Purpose |
|---------|---------|
| `mkdir -p ~/course/day02/{notes,logs,scripts}` | Create parent directories and subdirectories in one command |
| `ls -la ~/course/day02/` | List directory contents with permissions and hidden files |
| `cd ~/linux-junior-devops-bootcamp/days/day02` | Change to day02 directory |
| `ls` | List files in current directory |
| `find ~/course -type f` | Find all regular files in course directory |
| `grep -i "error" /var/log/syslog` | Search syslog for errors (case-insensitive) |
| `tail -n 20` | Show last 20 lines of output |
| `tar -czf backup.tar.gz ~/course/day02` | Create compressed archive of day02 directory |
| `tar -tvf backup.tar.gz` | List contents of archive without extracting |

## Output / Evidence

**mkdir -p ~/course/day02/{notes,logs,scripts}**

Output: (no output on success)

**ls -la ~/course/day02/**

Output:
total 12
drwxrwxr-x 5 affan-khan affan-khan 4096 Feb 22 02:02 .
drwxrwxr-x 3 affan-khan affan-khan 4096 Feb 22 02:02 ..
drwxrwxr-x 2 affan-khan affan-khan 4096 Feb 22 02:02 logs
drwxrwxr-x 2 affan-khan affan-khan 4096 Feb 22 02:02 notes
drwxrwxr-x 2 affan-khan affan-khan 4096 Feb 22 02:02 scripts

text

**cd ~/linux-junior-devops-bootcamp/days/day02**

Output: (no output)

**ls**

Output:
day02-cli-files-lab.md

text

**find ~/course -type f**

Output:
/home/affan-khan/course/day02/notes/lab_notes.txt

text

**grep -i "error" /var/log/syslog | tail -n 20**

Output:
2026-02-22T00:29:01.255390+01:00 affan-khan-VirtualBox org.gnome.Nautilus[13792]: MESA: error: ZINK: failed to choose pdev
2026-02-22T00:34:25.991427+01:00 affan-khan-VirtualBox tracker-miner-fs-3[14184]: (tracker-extract-3:14184): GLib-GIO-WARNING **: 00:34:25.988: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)
2026-02-22T00:34:36.572228+01:00 affan-khan-VirtualBox org.gnome.Nautilus[14214]: MESA: error: ZINK: failed to choose pdev
2026-02-22T00:39:21.909429+01:00 affan-khan-VirtualBox tracker-miner-fs-3[14518]: (tracker-extract-3:14518): GLib-GIO-WARNING **: 00:39:21.906: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)
2026-02-22T00:40:05.825101+01:00 affan-khan-VirtualBox org.gnome.Nautilus[14683]: MESA: error: ZINK: failed to choose pdev
2026-02-22T01:23:19.994038+01:00 affan-khan-VirtualBox gnome-shell[2400]: libinput error: event2 - AT Translated Set 2 keyboard: client bug: event processing lagging behind by 72ms, your system is too slow
2026-02-22T02:02:26.014059+01:00 affan-khan-VirtualBox tracker-miner-fs-3[17129]: (tracker-extract-3:17129): GLib-GIO-WARNING **: 02:02:26.011: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)

text

**tar -czf backup.tar.gz ~/course/day02**

Output:
tar: Removing leading `/' from member names

text

**tar -tvf backup.tar.gz**

Output:
rwxrwxr-x affan-khan/affan-khan 0 2026-02-22 02:02 home/affan-khan/course/day02/
drwxrwxr-x affan-khan/affan-khan 0 2026-02-22 02:02 home/affan-khan/course/day02/scripts/
drwxrwxr-x affan-khan/affan-khan 0 2026-02-22 02:02 home/affan-khan/course/day02/logs/
drwxrwxr-x affan-khan/affan-khan 0 2026-02-22 02:02 home/affan-khan/course/day02/notes/
-rw-rw-r-- affan-khan/affan-khan 0 2026-02-22 02:02 home/affan-khan/course/day02/notes/lab_notes.txt

text

## Validation

| Requirement | Validation method | Status | Evidence |
|-------------|------------------|--------|----------|
| Directory structure created | `ls -la ~/course/day02/` shows notes, logs, scripts | ✅ PASS | Three directories visible in ls output |
| File created in notes | `find ~/course -type f` returns lab_notes.txt | ✅ PASS | Path shows /notes/lab_notes.txt |
| Log investigation executed | `grep -i "error"` returns lines from syslog | ✅ PASS | Error lines displayed |
| Archive created | `tar -tvf backup.tar.gz` shows contents | ✅ PASS | Directory structure preserved in archive |

## Troubleshooting

**Issue 1:** Running `find ~/course` and getting "No such file or directory"

**Root cause:** The directory structure hadn't been created yet.

**Diagnosis:** Ran `ls ~/course` and saw directory didn't exist.

**Resolution:** Ran `mkdir -p ~/course/day02/{notes,logs,scripts}` first, then `find` worked.

**Lesson:** Always create directory structure before searching for files.

**Issue 2:** Tar warning "Removing leading `/' from member names"

**Root cause:** Tar removes leading slash from absolute paths as a safety feature. This prevents accidentally overwriting system files when extracting.

**Diagnosis:** This is a warning, not an error. The archive still works correctly.

**Resolution:** No action needed. The paths inside become relative (`home/affan-khan/course/...` instead of `/home/affan-khan/course/...`). This is safer for extraction.

**Lesson:** This warning is normal protection. Tar is preventing you from damaging your system.

**Issue 3:** Trying to run commands while nano was already occupying the terminal

**Root cause:** Nano takes over the terminal until you exit.

**Diagnosis:** Terminal was unresponsive to commands.

**Resolution:** Opened a second terminal tab/window for commands.

**Lesson:** Use multiple terminals: one for editing files, one for running commands.

## System Performance Investigation

**Observation:** My system feels slow

**Hypothesis:** If the system is slow, then logs will show errors about hardware processing or slow events

**Command used:** `grep -i "error" /var/log/syslog | tail -n 10`

**Evidence found:**
2026-02-22T01:23:19.994038+01:00 affan-khan-VirtualBox gnome-shell[2400]: libinput error: event2 - AT Translated Set 2 keyboard: client bug: event processing lagging behind by 72ms, your system is too slow

text

**Result:** Hypothesis confirmed. System logs show keyboard lag of 72ms.

## Lessons Learned

1. **mkdir -p** creates entire parent directory chains in one command. Without -p, you get "No such file or directory" error.

2. **Tar warning about removing leading slash** is a safety feature, not an error. It converts absolute paths to relative paths to prevent accidental system file overwrites during extraction.

3. **grep -i** ignores case sensitivity. "ERROR", "error", and "Error" all match.

4. **Pipe with tail** limits output to recent lines. grep alone on /var/log/syslog would return thousands of lines.

5. **tar -czf** does two jobs: -c creates the archive, -z compresses it. Without -z, the file is a bundle but not compressed.

6. **tail -f** is the industry standard for watching logs in real-time. It shows new lines as they appear.

7. **Absolute paths** work from anywhere. **Relative paths** are shorter but break if you change directories.


## Quiz Questions and Answers

**Q1. Difference between absolute and relative path?**

**My Answer:** Absolute path starts from root (/). Relative path starts from my current location (pwd).

**Q2. What does | (pipe) do in shell?**

**My Answer:** Pipe connects different tools together. Output of first command becomes input of second command.

**Q3. Command to search text in a file?**

**My Answer:** `grep`

**Q4. Purpose of tar?**

**My Answer:** Tar bundles files together. The `-z` flag does the actual compression to save disk space.

**Q5. Why use head and tail?**

**My Answer:** `head` checks initial logs or file headers. `tail` checks current or recent logs. `tail -f` watches logs in real-time.

