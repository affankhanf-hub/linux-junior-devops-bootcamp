# Day 4 - Processes, Services, Logs

## Theory Summary

1. **Process:** A program which is running right now examples Firefox, Ls, Nginx (When installed) e.t.c

2. **Service:** Is a process that runs in the background. It is managed by systemd and starts at boot examples Nginx, SSH and MySQL

3. **Troubleshooting protocol**
   - Do not restart blindly
   - Run 4 commands 
   - Hypothesize
   - Verify it is working

4. **ps aux | grep nginx** - Purpose: Check if nginx process is running

5. **systemctl status nginx** - Purpose: Check if service is enabled and running

6. **journalctl -u nginx --since "30 min ago"** - Purpose: See what happened in the last 30 minutes

7. **ss -tulpen | grep :80** - Purpose: Check who is listening on port 80

8. **SYSTEMD - The Boss of Your Linux System** - systemd = The first process that starts when Linux boots (PID 1) and manages EVERYTHING else

## Commands Used

| Command | Purpose |
|---------|---------|
| `ps aux | grep nginx` | Check if nginx process is running |
| `systemctl status nginx` | Check if service is enabled and running |
| `journalctl -u nginx --since "30 min ago"` | See what happened in the last 30 minutes |
| `ss -tulpen | grep :80` | Check who is listening on port 80 |

## Output / Evidence

### Command 1: ps aux | grep nginx - Is the process running
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day04$ ps aux | grep nginx
root 1260 0.0 0.0 11172 1944 ? Ss Feb27 0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
www-data 1262 0.0 0.1 12948 4588 ? S Feb27 0:00 nginx: worker process
affan-k+ 10536 0.0 0.0 9152 2324 pts/1 S+ 04:32 0:00 grep --color=auto nginx

text

**What This Tells You:**
- Line 1: ✅ nginx master process IS running (PID 1260, started Feb 27)
- Line 2: ✅ nginx worker process IS running (PID 1262)
- Line 3: ❌ This is just your grep command itself – ignore it

**Is nginx running?** Yes ✅ (you see master and worker processes)

**When was nginx started?** Feb 27 (look at the date in line 1)

**Who runs the master process?** root user

**Who runs the worker process?** www-data user (this is good for security)

### Command 2: systemctl status nginx - Check if service is enabled and running
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day04$ systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
Active: active (running) since Fri 2026-02-27 21:13:32 CET; 7h ago
Docs: man:nginx(8)
Process: 1247 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on;
Process: 1256 ExecStart=/usr/sbin/nginx -g daemon on; master_process on;
Main PID: 1260 (nginx)
Tasks: 2 (limit: 4601)
Memory: 3.1M (peak: 3.3M swap: 36.0K swap peak: 36.0K)
CPU: 54ms
CGroup: /system.slice/nginx.service
├─1260 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
└─1262 "nginx: worker process"

Feb 27 21:13:31 affan-khan-VirtualBox systemd[1]: Starting nginx.service - A high performance web server...
Feb 27 21:13:32 affan-khan-VirtualBox systemd[1]: Started nginx.service - A high performance web server.

text

### Command 3: journalctl -u nginx --since "30 min ago" - See what happened in the last 30 minutes
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day04$ journalctl -u nginx --since "30 min ago"
-- No entries --

text

### Command 4: ss -tulpen | grep :80
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day04$ ss -tulpen | grep :80
tcp LISTEN 0 511 0.0.0.0:80 0.0.0.0:* ino:11554 sk:8 cgroup:/system.slice/nginx.service <->
tcp LISTEN 0 511 [::]:80 [::]:* ino:11555 sk:c cgroup:/system.slice/nginx.service v6only:1 <->

text

## Lab Task 5: Put It All Together – Normal Operation

```bash
echo "=== COMMAND 1: ps aux | grep nginx ==="
ps aux | grep nginx

echo "=== COMMAND 2: systemctl status nginx ==="
systemctl status nginx | head -10

echo "=== COMMAND 3: journalctl -u nginx --since '5 min ago' ==="
sudo journalctl -u nginx --since "5 min ago" | tail -10

echo "=== COMMAND 4: ss -tulpen | grep :80 ==="
ss -tulpen | grep :80
Output:

text
=== COMMAND 1: ps aux | grep nginx ===
root        1260  0.0  0.0  11172  1944 ?        Ss   Feb27   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
www-data    1262  0.0  0.1  12948  4968 ?        S    Feb27   0:00 nginx: worker process
affan-k+   10626  0.0  0.1  19944  6000 pts/1    T    04:40   0:00 systemctl status nginx
affan-k+   10912  0.0  0.0   9152  2328 pts/1    S+   05:02   0:00 grep --color=auto nginx

=== COMMAND 2: systemctl status nginx ===
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Fri 2026-02-27 21:13:32 CET; 7h ago
       Docs: man:nginx(8)
    Process: 1247 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 1256 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 1260 (nginx)
      Tasks: 2 (limit: 4601)
     Memory: 3.0M (peak: 3.3M swap: 36.0K swap peak: 36.0K)
        CPU: 57ms

=== COMMAND 3: journalctl -u nginx --since '5 min ago' ===
[sudo] password for affan-khan: 
-- No entries --

=== COMMAND 4: ss -tulpen | grep :80 ===
tcp   LISTEN 0      511          0.0.0.0:80         0.0.0.0:*    ino:11554 sk:8 cgroup:/system.slice/nginx.service <->                       
tcp   LISTEN 0      511             [::]:80            [::]:*    ino:11555 sk:c cgroup:/system.slice/nginx.service v6only:1 <->               
Hypothesis and Assumptions
Failure Scenario Analysis
Command 1: ps aux | grep nginx

text
root        1260  0.0  0.0  11172  1944 ?        Ss   Feb27   0:00 nginx: master process /usr/sbin/nginx
affan-k+   10789  0.0  0.0   9152  2324 pts/1    S+   05:15   0:00 grep --color=auto nginx
Observation: Worker process is missing! Only master remains.

Command 2: systemctl status nginx

text
● nginx.service - A high performance web server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled)
     Active: failed (Result: exit-code) since Sat 2025-02-28 05:15:23 UTC
Observation: Service shows "failed"

Command 3: sudo journalctl -u nginx --since "1 min ago"

text
Feb 28 05:15:23 ubuntu nginx[1260]: nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
Observation: Logs show "Address already in use" on port 80

Command 4: ss -tulpen | grep :80

text
tcp   LISTEN 0   511   0.0.0.0:80   0.0.0.0:*   users:(("apache2",pid=2345))
Observation: Port 80 shows apache2, not nginx!

Failure observed:

nginx service shows "failed"

Worker process missing

Logs show "Address already in use"

Port 80 is owned by apache2

Root cause hypothesis: Another service (apache2) is using port 80, so nginx cannot bind to it. This is a port conflict.


What the failure was: Nginx service failed to start. The worker process was missing from ps aux output and systemctl status nginx showed "failed" instead of "active (running)".

What command confirmed it: systemctl status nginx showed "Active: failed (Result: exit-code)" and ss -tulpen | grep :80 confirmed that apache2 was using port 80 instead of nginx.

What the root cause was: Port conflict. Apache2 was already installed and running on the system, listening on port 80. Nginx could not bind to port 80 because it was already occupied by another web server.

What the final fix was: Stop apache2 with sudo systemctl stop apache2, disable it from starting at boot with sudo systemctl disable apache2, then restart nginx with sudo systemctl restart nginx.

Flags Reference
Command 1: ps aux | grep nginx
Flag	Full form	What it does
a	all	Shows processes from all users (not just yours)
u	user	Shows user-friendly format (who owns it, CPU%, memory%)
x	extended	Shows background processes (like services with no terminal)
Why these flags matter:

Without a → you'd only see your own processes

Without u → you wouldn't see who owns the process

Without x → you wouldn't see nginx (it runs in background)

Command 2: systemctl status nginx
Flag	What it does
No flags needed	status itself is the command, not a flag
| -l	Shows full output (not truncated)
| --no-pager	Shows all output at once (doesn't pause)
Example: systemctl status nginx --no-pager

Command 3: journalctl -u nginx --since "30 min ago"
Flag	Full form	What it does
-u	unit	Show logs for a specific service (nginx)
--since	–	Show logs from specified time
Other useful flags:
| -n 20 | Show last 20 lines |
| -f | Follow logs in real-time (like tail -f) |
| -p err | Show only errors |

Examples:

bash
journalctl -u nginx -n 20                 # Last 20 lines
journalctl -u nginx -f                    # Follow in real-time
journalctl -u nginx --since "5 min ago"   # Last 5 minutes
Command 4: ss -tulpen | grep :80
Flag	Full form	What it does
-t	tcp	Show TCP ports
-u	udp	Show UDP ports
-l	listening	Show only ports that are LISTENING
-p	process	Show which process is using the port
-e	extended	Show extra details
-n	numeric	Show port numbers (not service names)
What each flag gives you:

Flag	Without it	With it
-t	No TCP ports shown	Shows TCP
-u	No UDP ports shown	Shows UDP
-l	Shows all connections	Shows only listening (waiting) ports
-p	No process info	Shows process name and PID
-e	Basic info	Shows user, inode details
-n	Shows "http" instead of 80	Shows "80" (faster, no lookup)
Day 4 Quiz
Questions:

Command to list processes?

Command to inspect systemd service state?

Where are systemd logs commonly viewed?

Meaning of a process in D state (high level)?

Why monitor logs before restarting services?

My Answers:

Question 1: ps aux | grep nginx

Question 2: systemctl status nginx

✅ CORRECT! Perfect answer.

Question 3: journalctl -u nginx --since "30 min ago"

✅ CORRECT! This shows systemd logs for nginx.

Question 4: Uninterruptible sleep – the process is waiting for disk I/O and cannot be killed easily. Think of it as "stuck waiting for disk."

✅ CORRECT! (Not "dying" - D state means stuck waiting for disk)

Question 5: To know the exact issue. Logs contain the evidence of what went wrong. If you restart blindly, you destroy that evidence.

✅ CORRECT! Perfect answer.

Lessons Learned
The four diagnostic commands work together. Never skip any when troubleshooting.

ps aux flags matter: a = all users, u = user format, x = background processes. Without x, you won't see services.

ss -tulpen replaces old netstat command. The flags follow a pattern: t=TCP, u=UDP, l=LISTENING, p=process, e=extended, n=numeric.

Port conflicts are a common root cause. Always check ss -tulpen | grep :<port> before starting a web service.

D state (Uninterruptible sleep) means the process is waiting for disk I/O and cannot be killed. Not "dying" or "dead".

journalctl --since is powerful for recent troubleshooting. Use "5 min ago", "1 hour ago", or "yesterday".

Golden troubleshooting rule: Logs first, restart last. Restarting destroys the evidence you need to find the root cause.

systemd is PID 1 - the boss of everything. It manages all services and starts them at boot.




