## Theory Summary

### TCP 3-Way Handshake Steps

The TCP handshake has exactly 3 steps:
- Client sends SYN (synchronize)
- Server responds SYN-ACK (synchronize-acknowledge)
- Client sends ACK (acknowledge)

Only after these 3 steps does data transfer begin.

### Handshake is Like a Phone Call

- SYN = Dialing the number
- SYN-ACK = Person answering "Hello?"
- ACK = You saying "Hi, it's me"
- Data transfer = Actual conversation

### Connection Refused Meaning

"Connection refused" means:
- The server is reachable
- But the specific port is closed OR no service is listening
- The server actively rejected the connection

**Link to service not listening:** This error directly indicates that no service is running on that port. The server got your SYN packet but had no application to handle it.

### Timeout Meaning

"Timeout" means:
- Your computer sent SYN packet
- No response ever came back
- Packets are being dropped (firewall) or route is broken

**Link to firewall dropping packets:** A firewall silently drops packets without responding. Your computer keeps waiting until it gives up.

**Link to broken routing:** If the network path is broken (wrong gateway, down router), packets never reach the destination.

### DNS Error Meaning

"DNS error" means:
- The handshake never even started
- Your computer couldn't resolve the domain name to an IP address
- Like trying to call someone without having their number

### Quick Error Reference

| Error | What It Means | Likely Cause |
|-------|---------------|--------------|
| Connection refused | Server reached, port closed | Service not listening |
| Timeout | No response | Firewall blocking OR routing broken |
| DNS error | Name doesn't resolve | DNS configuration issue |

## Commands Used

| Command | Purpose |
|---------|---------|
| `ss -tulpen | grep :80` | Show what service is listening on port 80 |
| `curl -I http://localhost` | Test web server response (headers only) |
| `nc -zv 127.0.0.1 80` | Test if port 80 is open (handshake test) |

### Command Flags Reference

**ss -tulpen | grep :80**

| Flag | Full Form | What It Does |
|------|-----------|--------------|
| -t | TCP | Show TCP ports |
| -u | UDP | Show UDP ports |
| -l | Listening | Show only listening ports |
| -p | Process | Show which process is using the port |
| -e | Extended | Show extended information |
| -n | Numeric | Show port numbers (not service names) |

**curl -I http://localhost**

| Flag | Full Form | What It Does |
|------|-----------|--------------|
| -I | Head | Fetch only the HTTP headers, not the full page |

**nc -zv 127.0.0.1 80**

| Flag | Full Form | What It Does |
|------|-----------|--------------|
| -z | Zero I/O | Just test connection, don't send data |
| -v | Verbose | Show detailed output |

## Output / Evidence

### LAB TASK 1: ss -tulpen | grep :80

```bash
$ ss -tulpen | grep :80
tcp   LISTEN 0      511          0.0.0.0:80         0.0.0.0:*    ino:11906 sk:a cgroup:/system.slice/nginx.service
tcp   LISTEN 0      511             [::]:80            [::]:*    ino:11907 sk:e cgroup:/system.slice/nginx.service v6only:1
What this shows: nginx is listening on port 80 (both IPv4 and IPv6).

LAB TASK 2: curl -I http://localhost
bash
$ curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 09 Mar 2026 22:07:14 GMT
Content-Type: text/html
Content-Length: 10671
What this shows: Web server is working and responding with 200 OK.

LAB TASK 3: nc -zv 127.0.0.1 80
bash
$ nc -zv 127.0.0.1 80
Connection to 127.0.0.1 80 port [tcp/http] succeeded!
What this shows: Port 80 is open and accepting connections.

LAB TASK 4: All 3 Commands Together
bash
=== TASK 4: Complete Test ===

1. ss -tulpen | grep :80
tcp   LISTEN 0      511          0.0.0.0:80         0.0.0.0:*    ino:11906 sk:a cgroup:/system.slice/nginx.service
tcp   LISTEN 0      511             [::]:80            [::]:*    ino:11907 sk:e cgroup:/system.slice/nginx.service v6only:1

2. curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)

3. nc -zv 127.0.0.1 80
Connection to 127.0.0.1 80 port [tcp/http] succeeded!
After Stopping nginx (Failure Scenario)
bash
$ sudo systemctl stop nginx

$ ss -tulpen | grep :80
(no output)

$ curl -I http://localhost
curl: (7) Failed to connect to localhost port 80: Connection refused

$ nc -zv 127.0.0.1 80
nc: connect to 127.0.0.1 port 80 (tcp) failed: Connection refused

Validation
What I Tested	Command	Expected Result	My Result	Status
Service listening on port 80	ss -tulpen | grep :80	See nginx process	nginx found	✅ PASS
Web server response	curl -I http://localhost	200 OK	200 OK	✅ PASS
Port 80 handshake	nc -zv 127.0.0.1 80	succeeded	succeeded	✅ PASS
After stopping nginx	All 3 commands	Connection refused	Connection refused	✅ PASS
Error to Root Cause Mapping
Error	What It Means	Likely Cause
Connection refused	Server reached, port closed	Service not listening
Timeout	No response	Firewall dropping OR routing broken
DNS error	Name doesn't resolve	DNS configuration issue
Troubleshooting
Scenario 1: Service is Listening But Not Responding
Observation:

bash
$ ss -tulpen | grep :80
tcp   LISTEN 0   511   0.0.0.0:80   users:(("nginx",pid=1234))

$ nc -zv 127.0.0.1 80
nc: connect to 127.0.0.1 port 80 (tcp) failed: Connection refused
What this means: nginx is listening (ss shows it) but not responding (nc fails). The service is hung.

Solution: sudo systemctl restart nginx

Scenario 2: No Service Listening
Observation:

bash
$ ss -tulpen | grep :80
(no output)
What this means: No service is installed or running on port 80.

Solution: Install and start nginx: sudo apt install -y nginx && sudo systemctl start nginx

Scenario 3: Firewall Dropping Packets
Observation:

bash
$ ss -tulpen | grep :80
tcp   LISTEN 0   511   0.0.0.0:80   users:(("nginx",pid=1234))

$ nc -zv 127.0.0.1 80
(no response - timeout after long wait)
What this means: Service is listening but firewall is blocking the connection.

Solution: sudo ufw allow 80/tcp

Real Troubleshooting Example
The Problem: A user reports "I can't access the company website at http://intranet"

Step 1 - Check if service is listening:

bash
ss -tulpen | grep :80
If no output → Service not listening (Connection refused)

Step 2 - Check if firewall is blocking:

bash
sudo ufw status
If firewall active and port 80 not allowed → Timeout

Step 3 - Check routing:

bash
ip route
ping -c 2 10.0.2.2
If ping to gateway fails → Broken routing (Timeout)

The Fix based on what you find:

Finding	Problem	Fix
ss shows no output	Service not listening	sudo systemctl start nginx
ufw active without port 80	Firewall dropping packets	sudo ufw allow 80/tcp
ping to gateway fails	Broken routing	Check network configuration

Lessons Learned

Connection refused = service not listening. The server got your request but had no application to handle it.

Timeout = firewall dropping packets OR broken routing. Your packet never reached the destination or the response never came back.

DNS error = handshake never starts. You don't even know where to send the SYN packet.

The three commands work together: ss shows who should be answering. nc actually tests the handshake. curl tests the full application response.

ss shows nginx listening on port 80 - both statements are true. nginx is the service, port 80 is the door it stands at.

Only one service can listen on a port at a time. If you try to start apache while nginx is on port 80, you get "Address already in use".

In WSL, localhost testing works fine. You do not need a VM for Day 9 because you are testing 127.0.0.1.

Difference between ss and nc: ss shows who SHOULD be answering (passive). nc actually TESTS if they DO answer (active). ss can show a service running, but nc might still fail if service is hung.

Quiz Answers

Question	My Answer
1. Explain TCP 3-way handshake	Client-Server-Client 3 way communication (SYN → SYN-ACK → ACK)
2. Difference between Connection refused and Timeout	Connection refused = service not running. Timeout = firewall blocking or network issue
3. What does nc -zv localhost 80 Connection refused mean?	Data is not being transmitted. Nginx not installed or hung up
4. What does curl -I http://localhost 200 OK tell you?	TCP handshake established successfully
5. What does ss -tulpen show?	Which ports are listening
6. ss shows nginx but nc gets Connection refused?	Service is hung - data not being transmitted
7. What do all 3 commands together tell you?	Whether handshake is established or not
8. Walk through troubleshooting steps	Check ss for listening, nc for handshake, curl for response
9. What does -z flag do in nc?	Zero I/O - just test connection, don't send data
10. What does -I flag do in curl?	Fetch only headers
11. Can ping 8.8.8.8 but not google.com?	DNS server down
12. What is port 80 and why important?	Port where web servers listen. Without it, TCP handshake cannot take place
