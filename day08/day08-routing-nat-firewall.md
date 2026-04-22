## Theory Summary

1. **Routing table** tells your computer how to send packets – which path to use for different destinations.

2. **Route selection** works on "most specific route wins" – if there's an exact match for a destination, use that; otherwise use default route.

3. **Default route** (usually default via 192.168.1.1) handles all destinations not specifically listed in routing table.

4. **NAT (Network Address Translation)** converts private IPs (like 192.168.1.x) to public IPs so multiple devices can share one internet connection.

5. **NAT conserves public IP space** – without it, every device would need its own public IP.

6. **NAT hides internal network topology** – outside world sees only your router's public IP, not your individual devices.

7. **Beginner note:** NAT issues often appear as "local network works, but internet fails" – can ping other devices at home but not 8.8.8.8.

8. **Stateful firewall** tracks connection state – if you initiate a connection, it allows return traffic automatically.

9. **Firewall best practice:** default deny incoming, default allow outgoing, only open specific ports you need (SSH 22, HTTP 80, HTTPS 443).

10. **Verification quick test:** check listening services with `ss -tulpen`, verify rules with `ufw status`, test with `curl`.

## Commands Used

| Command | Purpose |
|---------|---------|
| `ip a | grep inet` | Check VM's private IP address |
| `curl ifconfig.me` | Check public IP address |
| `which ufw` | Verify UFW is installed |
| `sudo ufw status` | Check current firewall status |
| `sudo ufw default deny incoming` | Set default policy to block incoming traffic |
| `sudo ufw default allow outgoing` | Set default policy to allow outgoing traffic |
| `sudo ufw allow 22/tcp` | Allow SSH traffic on port 22 |
| `sudo ufw allow 80/tcp` | Allow HTTP traffic on port 80 |
| `sudo ufw allow 443/tcp` | Allow HTTPS traffic on port 443 |
| `sudo ufw enable` | Enable the firewall |
| `sudo ufw status verbose` | Show detailed firewall status |
| `ss -tulpen | grep -E ":22|:80|:443"` | Check listening services on ports 22, 80, 443 |
| `curl -I http://localhost` | Test web server connectivity |
| `nc -zv localhost 22` | Test SSH port connection |

## Output / Evidence

### VM Private IP
```bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ ip a | grep inet
inet 127.0.0.1/8 scope host lo
inet6 ::1/128 scope host noprefixroute 
inet 10.0.2.200/24 brd 10.0.2.255 scope global noprefixroute enp0s3
inet 10.0.2.15/24 brd 10.0.2.255 scope global secondary dynamic noprefixroute
Public IP
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ curl ifconfig.me
109.104.32.217
Check UFW Installation
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ which ufw
/usr/sbin/ufw
Initial Firewall Status
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw status
Status: inactive
Set Default Policies
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw default deny incoming
Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw default allow outgoing
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)
Allow Essential Services
bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
Enable and Verify Firewall
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw enable
Firewall is active and enabled on system startup
Firewall Status After Enable
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
443/tcp                    ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
443/tcp (v6)               ALLOW IN    Anywhere (v6)
Firewall Verification Quick Test
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ ss -tulpen | grep -E ":22|:80|:443"
tcp   LISTEN 0      4096         0.0.0.0:22         0.0.0.0:*    ino:8512 sk:9 cgroup:/system.slice/ssh.socket
tcp   LISTEN 0      511          0.0.0.0:80         0.0.0.0:*    ino:11906 sk:a cgroup:/system.slice/nginx.service
tcp   LISTEN 0      4096            [::]:22            [::]:*    ino:8516 sk:d cgroup:/system.slice/ssh.socket v6only:1
tcp   LISTEN 0      511             [::]:80            [::]:*    ino:11907 sk:e cgroup:/system.slice/nginx.service v6only:1
Note: Port 443 only inactive as HTTPS is not configured on the web server.

Web Server Test
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 09 Mar 2026 15:38:02 GMT
Content-Type: text/html
Content-Length: 10671
Last-Modified: Wed, 19 Nov 2025 12:17:57 GMT
Connection: keep-alive
ETag: "691db575-29af"
Accept-Ranges: bytes
SSH Port Test
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ nc -zv localhost 22
Connection to localhost (127.0.0.1) 22 port [tcp/ssh] succeeded!
Blocked Port Test
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ nc -zv localhost 8080
nc: connect to localhost (127.0.0.1) port 8080 (tcp) failed: Connection refused
Validation
Requirement	Validation method	Status	Evidence
UFW installed	which ufw returns /usr/sbin/ufw	✅ PASS	Shows installation path
Firewall enabled	sudo ufw status shows active	✅ PASS	Status: active
Default policies set	sudo ufw status verbose shows deny incoming, allow outgoing	✅ PASS	Default: deny (incoming), allow (outgoing)
SSH port allowed	sudo ufw status shows 22/tcp ALLOW IN	✅ PASS	22/tcp listed
HTTP port allowed	sudo ufw status shows 80/tcp ALLOW IN	✅ PASS	80/tcp listed
Web server accessible	curl -I http://localhost returns 200 OK	✅ PASS	HTTP/1.1 200 OK
SSH port reachable	nc -zv localhost 22 succeeds	✅ PASS	Connection succeeded
Blocked port unreachable	nc -zv localhost 8080 fails	✅ PASS	Connection refused
Troubleshooting
Scenario 1: Firewall Blocking Web Traffic
Problem: Web server not accessible

bash
curl -I http://localhost
curl: (7) Failed to connect to localhost port 80: Connection refused

ss -tulpen | grep :80
tcp   LISTEN 0      511    0.0.0.0:80    0.0.0.0:*    users:(("nginx",pid=1234))

sudo ufw status verbose
Status: active
Default: deny (incoming), allow (outgoing)
Observation: Web server is listening (ss shows it), but curl cannot connect.

Hypothesis: Firewall is blocking port 80 because no allow rule exists.

Fix applied:

bash
sudo ufw allow 80/tcp
curl -I http://localhost
Result: HTTP/1.1 200 OK

Scenario 2: Forgot to Allow Port Before Enabling Firewall
Problem: After setting up firewall, web server is unreachable.

Commands run:

bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw enable
Verification shows failure:

bash
curl -I http://localhost
curl: (7) Failed to connect to localhost port 80: Connection refused

ss -tulpen | grep :80
tcp   LISTEN 0      511    0.0.0.0:80    0.0.0.0:*    users:(("nginx",pid=1234))
Observation: Web server is listening on port 80, but curl cannot connect.

Hypothesis: Forgot to add the allow rule for port 80 before enabling the firewall.

Fix applied:

bash
sudo ufw allow 80/tcp
curl -I http://localhost
Result: HTTP/1.1 200 OK

REQUIRED CORRECTIONS (As per feedback)
NAT Explanation - Where NAT is happening in your VM/lab setup
Where NAT is happening: In my VirtualBox lab setup, NAT is happening at two levels. First, VirtualBox has a built-in NAT engine that translates my VM's private IP (10.0.2.15) to my host machine's IP. Second, my home router performs another NAT translation from my host machine's private IP to my ISP-assigned public IP (109.104.32.217). This is called "double NAT" when running VMs.

How local traffic differs from internet-bound traffic:

Local traffic (within VM/host): When I ping another device on my home network (like 192.168.1.5), the packet stays within the local network. No NAT translation occurs because both devices share the same private IP range.

Internet-bound traffic (to 8.8.8.8): When I ping Google's DNS server, the packet must go through NAT. My router replaces my private IP (192.168.1.x) with my public IP (109.104.32.217) before sending it to the internet. The response comes back to my public IP, and the router remembers which internal device requested it.

What default deny incoming protects against: Default deny incoming blocks any traffic that originates from the internet and tries to reach my computer without me asking for it. For example, if a hacker scans my public IP and tries to connect to port 22 (SSH), the firewall blocks it because I never initiated that connection. However, if I start a connection to a website, the firewall allows the website's response because it's "related" to my outgoing request. This is stateful firewall behavior.

Hypothesis Practice
Scenario 1: Firewall Blocking Web Traffic
Problem: curl -I http://localhost returns "Connection refused"

Observation: ss -tulpen | grep :80 shows nginx listening on port 80

Hypothesis: Firewall is blocking port 80

Test: sudo ufw status verbose to check if port 80 is allowed

Fix: sudo ufw allow 80/tcp

Expected result: HTTP/1.1 200 OK

Scenario 2: Forgot to Allow Port Before Enabling
Problem: After sudo ufw enable, website is down

Observation: Web server is listening but curl cannot connect

Hypothesis: Forgot to add allow rule for port 80 before enabling firewall

Test: sudo ufw status verbose to see if port 80 is listed

Fix: sudo ufw allow 80/tcp

Flags Reference
UFW Command Flags
Part	What it means
sudo	Run as root (need permission)
ufw	Uncomplicated Firewall
default	Set default policy
allow	Create an allow rule
deny	Block traffic
22/tcp	Port 22, protocol TCP
80/tcp	Port 80, protocol TCP
443/tcp	Port 443, protocol TCP
verbose	Show detailed information
SS Command Flags
Flag	What it does
-t	Show TCP ports
-u	Show UDP ports
-l	Show only listening ports
-p	Show process using the port
-e	Show extended details
-n	Show numeric port numbers


Day 8 Quiz
Questions:

What is NAT used for?

What command enables UFW?

Why default deny inbound?

What is stateful firewall behavior?

What does routing table decide?

My Answers:

Question 1: NAT is used to translate private IPs to Public IPs (network translator)

✅ CORRECT! NAT allows multiple devices to share one public IP.

Question 2: sudo ufw enable

✅ CORRECT!

Question 3: It stops unsolicited traffic coming inside

✅ CORRECT! Default deny inbound means you only allow what you explicitly need.

Question 4: Stateful firewall tracks the state of connections. If I initiate a connection to google.com, the firewall remembers this and automatically allows the return traffic. But it blocks unsolicited incoming traffic that I didn't request.

✅ CORRECT

Question 5: Table decides which route to be adopted for a particular traffic. Whether to use local or default

✅ CORRECT!

Interview Questions
Q1: Explain what NAT is and why we need it in simple terms.
My answer: Network address translator translates private IP into public IP as every private IP needs a public IP to communicate with outside network.

✅ CORRECT!

Q2: What's the difference between a default route and a specific route in the routing table?
My answer: Default route is the gateway from where traffic is routed to the outside network. Specific route is the known route or within the local network.

✅ CORRECT!

Q3: You just enabled UFW on a server and now the website is down. The web server is still running. What's the first thing you check?
My answer: I would check if I allowed port 80 in the firewall using sudo ufw status verbose. If port 80 is not listed, I need to add it with sudo ufw allow 80/tcp.

✅ CORRECT!

Q4: Why is "default deny inbound" considered a best practice for firewalls?
My answer: It is secure and protects network from malicious activities.

✅ CORRECT!

Q5: What does "stateful" mean when we talk about a stateful firewall?
My answer: Stateful firewall tracks the state of connections. If I initiate a connection, the firewall remembers this and automatically allows the return traffic. But it blocks unsolicited incoming traffic that I didn't request.

✅ CORRECT!

Q6: Your local network works (you can ping other devices at home), but you cannot ping 8.8.8.8. What could be wrong?
My answer: Since local network works but internet doesn't, this suggests a NAT or upstream routing issue. The problem is likely on the router or ISP side, not my computer. DNS is not the issue because ping uses IP address.

✅ CORRECT!

Q7: Walk me through the steps to set up a basic firewall on a new Ubuntu server using UFW.
My answer:

text
Step 1: sudo ufw default deny incoming
Step 2: sudo ufw default allow outgoing
Step 3: sudo ufw allow 22/tcp   (SSH)
Step 4: sudo ufw allow 80/tcp   (HTTP)
Step 5: sudo ufw allow 443/tcp  (HTTPS)
Step 6: sudo ufw enable
Step 7: sudo ufw status verbose
✅ CORRECT!

Q8: What commands would you use to verify your firewall is working correctly?
My answer:

text
1. sudo ufw status verbose      (check firewall rules)
2. ss -tulpen | grep -E ":22|:80|:443"   (check services are listening)
3. curl -I http://localhost     (test web server)
4. nc -zv localhost 22          (test SSH port)
5. nc -zv localhost 8080        (test a blocked port - should fail)
✅ CORRECT!

Q9: If you have two routes to the same destination, one with metric 100 and one with metric 50, which one gets used and why?
My answer: The route with metric 50 gets used because lower metric = higher priority. The routing table always chooses the path with the lowest metric value.

✅ CORRECT!

Q10: You enabled UFW and immediately got disconnected from SSH. What went wrong and how could you have prevented it?
My answer: What went wrong: I forgot to allow SSH (port 22) BEFORE enabling the firewall. When I enabled UFW with default deny incoming, it blocked my SSH connection.

How to prevent: Always run sudo ufw allow 22/tcp BEFORE sudo ufw enable. This ensures SSH is allowed before the firewall turns on.

✅ CORRECT! (Note: SSH uses port 22, not port 20)

Lessons Learned
NAT happens in multiple places – In VirtualBox lab, double NAT occurs (VirtualBox NAT + home router NAT). Understanding where NAT occurs helps troubleshoot "local works, internet fails" issues.

Local traffic vs internet traffic – Local traffic stays within private IP range and does not get NAT translated. Internet-bound traffic must go through NAT to get a public IP.

Default deny incoming protects against – Unsolicited incoming connections from the internet. It blocks hackers from scanning or connecting to your open ports unless you initiated the connection first.

Always allow SSH before enabling firewall – If you enable UFW without allowing port 22 first, you will lock yourself out of the server.

Lower metric = higher priority – When multiple routes exist to the same destination, the route with the lowest metric number is chosen.

Stateful firewall tracks connections – It remembers your outgoing requests and automatically allows the return traffic, but blocks unsolicited incoming traffic.

UFW verification requires multiple commands – Check rules with ufw status verbose, check listening services with ss, test ports with curl and nc.

NAT conserves public IP addresses – Without NAT, every device would need its own public IP address, which is not feasible.

