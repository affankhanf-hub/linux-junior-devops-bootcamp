10 Points

1. Routing table tells your computer how to send packets – which path to use for different destinations.

2. Route selection works on "most specific route wins" – if there's a exact match for a destination, use that; otherwise use default route.

3. Default route (usually default via 192.168.1.1) handles all destinations not specifically listed in routing table.
  
4. NAT (Network Address Translation) converts private IPs (like 192.168.1.x) to public IPs so multiple devices can share one internet connection.

5. NAT conserves public IP space – without it, every device would need its own public IP.

6. NAT hides internal network topology – outside world sees only your router's public IP, not your individual devices.

7. Beginner note: NAT issues often appear as "local network works, but internet fails" – can ping other devices at home but not 8.8.8.8.

8. Stateful firewall tracks connection state – if you initiate a connection, it allows return traffic automatically.

9. Firewall best practice: default deny incoming, default allow outgoing, only open specific ports you need (SSH 22, HTTP 80, HTTPS 443).

10. Verification quick test: check listening services with ss -tulpen, verify rules with ufw status, test with curl.

Practical lab


COMMAND FLAGS


sudo ufw default and sudo ufw allow

Part	What it means
sudo	Run as root (need permission)
ufw	Uncomplicated Firewall
default	Set default policy
allow	Create an allow rule
deny	Block traffic
22/tcp	Port 22, protocol TCP
80/tcp	Port 80, protocol TCP
443/tcp	Port 443, protocol TCP

sudo ufw status verbose
Flag	What it does
verbose	Show detailed information

VM's private IP
ip a | grep inet

inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host noprefixroute 
    inet 10.0.2.200/24 brd 10.0.2.255 scope global noprefixroute enp0s3
    inet 10.0.2.15/24 brd 10.0.2.255 scope global secondary dynamic noprefixrout

Public IP

curl ifconfig.me

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ curl ifconfig.me
109.104.32.217


Lab Task 3: Check Current Firewall Status

which ufw

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ which ufw
/usr/sbin/ufw

sudo ufw status

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw status
[sudo] password for affan-khan: 
Status: inactive


Lab Task 4: Set Default Policies

sudo ufw default deny incoming

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw default deny incoming
Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)


sudo ufw default allow outgoing

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw default allow outgoing
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)


Lab Task 5: Allow Essential Services

# Allow SSH (so you don't lock yourself out)
sudo ufw allow 22/tcp

# Allow web traffic
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Check rules so far
sudo ufw status verbose


affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw enable
Firewall is active and enabled on system startup

sudo ufw status verbose

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
80/tcp (Apache)            ALLOW IN    Anywhere                  
22/tcp                     ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
443/tcp                    ALLOW IN    Anywhere                  
80/tcp (Apache (v6))       ALLOW IN    Anywhere (v6)             
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
443/tcp (v6)               ALLOW IN    Anywhere (v6)             



FIREWALL VERIFICATION QUICK TEST

ss -tulpen | grep -E ":22|:80|:443"


affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ ss -tulpen | grep -E ":22|:80|:443"
tcp   LISTEN 0      4096         0.0.0.0:22         0.0.0.0:*    ino:8512 sk:9 cgroup:/system.slice/ssh.socket <->                           
tcp   LISTEN 0      511          0.0.0.0:80         0.0.0.0:*    ino:11906 sk:a cgroup:/system.slice/nginx.service <->                       
tcp   LISTEN 0      4096            [::]:22            [::]:*    ino:8516 sk:d cgroup:/system.slice/ssh.socket v6only:1 <->                  
tcp   LISTEN 0      511             [::]:80            [::]:*    ino:11907 sk:e cgroup:/system.slice/nginx.service v6only:1 <->              

443 only inactive as HTTPS is not configured


Step 2: Confirm allow rule with ufw status

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
80/tcp (Apache)            ALLOW IN    Anywhere                  
22/tcp                     ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
443/tcp                    ALLOW IN    Anywhere                  
80/tcp (Apache (v6))       ALLOW IN    Anywhere (v6)             
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
443/tcp (v6)               ALLOW IN    Anywhere (v6)          

Step 3: Test from client with curl

# Test web server
curl -I http://localhost

# Test SSH port (connection test)
nc -zv localhost 22

# Test a port that should be BLOCKED (optional)
nc -zv localhost 8080

b server
curl -I http://localhost

# Test SSH port (connection test)
nc -zv localhost 22

# Test a port that should be BLOCKED (optional)
nc -zv localhost 8080
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 09 Mar 2026 15:38:02 GMT
Content-Type: text/html
Content-Length: 10671
Last-Modified: Wed, 19 Nov 2025 12:17:57 GMT
Connection: keep-alive
ETag: "691db575-29af"
Accept-Ranges: bytes

Connection to localhost (127.0.0.1) 22 port [tcp/ssh] succeeded!
nc: connect to localhost (127.0.0.1) port 8080 (tcp) failed: Connection ref

HYPOTHESIS PRACTICE
Routing, NAT, and Firewall Scenarios

SCENARIO 1: Firewall Blocking Web Traffic

curl -I http://localhost

curl: (7) Failed to connect to localhost port 80: Connection refused

ss -tulpen | grep :80

tcp   LISTEN 0      511    0.0.0.0:80    0.0.0.0:*    users:(("nginx",pid=1234))

sudo ufw status verbose

Status: active
Default: deny (incoming), allow (outgoing)

My response sudo ufw allow 80/tcp

AFTER RUNNING YOUR FIX

# Add the rule
sudo ufw allow 80/tcp

# Verify it's added
sudo ufw status verbose

# Test again
curl -I http://localhost

Expected result: HTTP/1.1 200 OK


THE SCENARIO 2

sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw enable

THE PROBLEM

You run the verification test:

curl -I http://localhost

curl: (7) Failed to connect to localhost port 80: Connection refused

ss -tulpen | grep :80

tcp   LISTEN 0      511    0.0.0.0:80    0.0.0.0:*    users:(("nginx",pid=1234))

Observation: Web server is listening on port 80 (ss shows it), 
             but curl cannot connect.

Hypothesis: I forgot to add the allow rule for port 80 before enabling the firewall.

Test: Check with `sudo ufw status verbose` to see if port 80 is listed.

Fix: If port 80 is not listed, run `sudo ufw allow 80/tcp` and test again with `curl -I http://localhost`.

sudo ufw status verbose

sudo ufw allow 80/tcp

curl -I http://localhost


Day 8 Quiz
1. What is NAT used for?
2. What command enables UFW?
3. Why default deny inbound?
4. What is stateful firewall behavior?
5. What does routing table decide?

My responses

1. Nat is used to translate private IPs to Public Ips (network trnaslator)
2. Sudo ufw enable
3. It stops unsolicited traffic coming inside
4. Dont know
5. Table decided which route to be adopted for a particular traffic. Whether to use local or default


Your Answers - Complete
#	Question	Your Answer	Status
1	What is NAT used for?	"Nat is used to translate private IPs to Public Ips (network translator)"	✅ CORRECT
2	What command enables UFW?	"Sudo ufw enable"	✅ CORRECT
3	Why default deny inbound?	"It stops unsolicited traffic coming inside"	✅ CORRECT
4	What is stateful firewall behavior?	"Dont know"	❌ Needs review
5	What does routing table decide?	"Table decided which route to be adopted for a particular traffic. Whether to use local or default"	✅ CORRECT


Interview section


DAY 8 - Interview Questions
Routing, NAT, and Firewall
Question 1

"Explain what NAT is and why we need it in simple terms."

Your answer:
Question 2

"What's the difference between a default route and a specific route in the routing table?"

Your answer:
Question 3

"You just enabled UFW on a server and now the website is down. The web server is still running. What's the first thing you check?"

Your answer:
Question 4

"Why is 'default deny inbound' considered a best practice for firewalls?"

Your answer:
Question 5

"What does 'stateful' mean when we talk about a stateful firewall?"

Your answer:
Question 6

"Your local network works (you can ping other devices at home), but you cannot ping 8.8.8.8. What could be wrong?"

Your answer:
Question 7

"Walk me through the steps to set up a basic firewall on a new Ubuntu server using UFW."

Your answer:
Question 8

"What commands would you use to verify your firewall is working correctly?"

Your answer:
Question 9

"If you have two routes to the same destination, one with metric 100 and one with metric 50, which one gets used and why?"

Your answer:
Question 10

"You enabled UFW and immediately got disconnected from SSH. What went wrong and how could you have prevented it?"

Your answer:

Ready when you are! Pick any question and give your answer.
1 Network address ttranslator translates private IP into public IP as every private ip needs a public ip to communicate with outside network

2. Default route is the gateway from where traffic is routed to the outside network.
Specific route is the known route or within the local network

3.


4. It is secure and protects network from malicious activities
5. Deny incoming, allow outgoing and allow ports
6. DNS server might be down ,router is not connecting with IP or firewall issue
7.
Great Answers! Let Me Give You Feedback
Question 1

Your answer: "Network address translator translates private IP into public IP as every private ip needs a public ip to communicate with outside network"

✅ CORRECT! Simple and accurate. NAT allows multiple devices to share one public IP.
Question 2

Your answer: "Default route is the gateway from where traffic is routed to the outside network. Specific route is the known route or within the local network"

✅ CORRECT! Perfect explanation:

    Default route = "everything else goes here"

    Specific route = exact match for local destinations

Question 3

(You left this blank)

Suggested answer: "I would check if I allowed port 80 in the firewall using sudo ufw status verbose. If port 80 is not listed, I need to add it with sudo ufw allow 80/tcp."
Question 4

Your answer: "It is secure and protects network from malicious activities"

✅ CORRECT! Default deny inbound means you only allow what you explicitly need, blocking everything else by default.
Question 5

Your answer: "Deny incoming, allow outgoing and allow ports"

⚠️ PARTIALLY CORRECT – This describes firewall policy, not what "stateful" means.

Better answer: "Stateful firewall tracks the state of connections. If I initiate a connection to google.com, the firewall remembers this and automatically allows the return traffic. But it blocks unsolicited incoming traffic that I didn't request."
Question 6

Your answer: "DNS server might be down, router is not connecting with IP or firewall issue"

⚠️ PARTIALLY CORRECT – But the question says "local network works (can ping other devices) but cannot ping 8.8.8.8".

Since you can ping other local devices but NOT 8.8.8.8:

    ✅ Router issue (maybe)

    ✅ Firewall issue (maybe)

    ❌ DNS is NOT the issue (ping uses IP, not domain)

Better answer: "Since local network works but internet doesn't, this suggests a NAT or upstream routing issue. The problem is likely on the router or ISP side, not my computer."
Question 7

(You left this blank)

Suggested answer: "First, I'd set default policies with sudo ufw default deny incoming and sudo ufw default allow outgoing. Then I'd allow essential services like sudo ufw allow 22/tcp for SSH, sudo ufw allow 80/tcp for HTTP, and sudo ufw allow 443/tcp for HTTPS. Finally, I'd enable the firewall with sudo ufw enable and verify with sudo ufw status verbose."
Questions You Still Need to Answer

    Q3: What's the first thing you check when website goes down after enabling firewall?

    Q7: Walk me through setting up UFW step by step.

    Q8: What commands verify firewall is working?

    Q9: Route metric question

    Q10: SSH disconnect after enabling firewall

Want to try any of these?
7 The 8 commands which we used
8. Sudo ufw verbose
9.
10. The port 20 got disabled. sudo ufw allow 20/tcp

You give me answers of remaining questions
Here Are the Answers for Remaining Questions
Question 7: "Walk me through the steps to set up a basic firewall on a new Ubuntu server using UFW."

Answer:
text

Step 1: sudo ufw default deny incoming
Step 2: sudo ufw default allow outgoing
Step 3: sudo ufw allow 22/tcp   (SSH)
Step 4: sudo ufw allow 80/tcp   (HTTP)
Step 5: sudo ufw allow 443/tcp  (HTTPS)
Step 6: sudo ufw enable
Step 7: sudo ufw status verbose

These are the 7 commands from the curriculum.
Question 8: "What commands would you use to verify your firewall is working correctly?"

Answer:
text

1. sudo ufw status verbose      (check firewall rules)
2. ss -tulpen | grep -E ":22|:80|:443"   (check services are listening)
3. curl -I http://localhost     (test web server)
4. nc -zv localhost 22          (test SSH port)
5. nc -zv localhost 8080        (test a blocked port - should fail)

This is the verification quick test from the curriculum.
Question 9: "If you have two routes to the same destination, one with metric 100 and one with metric 50, which one gets used and why?"

Answer:
"The route with metric 50 gets used because lower metric = higher priority. The routing table always chooses the path with the lowest metric value."
Question 10: "You enabled UFW and immediately got disconnected from SSH. What went wrong and how could you have prevented it?"

Answer:
"What went wrong: I forgot to allow SSH (port 22) BEFORE enabling the firewall. When I enabled UFW with default deny incoming, it blocked my SSH connection.

How to prevent: Always run sudo ufw allow 22/tcp BEFORE sudo ufw enable. This ensures SSH is allowed before the firewall turns on."
Summary of Your Answers vs Correct Answers
Q	Your Answer	Correct Answer
7	"The 8 commands which we used"	The 7 UFW commands in order
8	"Sudo ufw verbose"	sudo ufw status verbose, ss, curl, nc
9	(blank)	Lower metric (50) wins
10	"The port 20 got disabled. sudo ufw allow 20/tcp"	Forgot to allow SSH (port 22) before enabling

Note: SSH uses port 22, not port 20. Port 20 is for FTP data transfer.

