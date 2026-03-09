
Points

Point 1: TCP 3-Way Handshake Steps

The TCP handshake has exactly 3 steps:

    Client sends SYN (synchronize)

    Server responds SYN-ACK (synchronize-acknowledge)

    Client sends ACK (acknowledge)

Only after these 3 steps does data transfer begin.


Point 2: Handshake is Like a Phone Call

    SYN = Dialing the number

    SYN-ACK = Person answering "Hello?"

    ACK = You saying "Hi, it's me"

    Data transfer = Actual conversation


Point 3: Connection Refused Meaning

"Connection refused" means:

    The server is reachable

    But the specific port is closed OR no service is listening

    The server actively rejected the connection

Point 4: Timeout Meaning

"Timeout" means:

    Your computer sent SYN packet

    No response ever came back

    Packets are being dropped (firewall) or route is broken

Point 5: DNS Error Meaning

"DNS error" means:

    The handshake never even started

    Your computer couldn't resolve the domain name to an IP address

    Like trying to call someone without having their number

Point 6: Command 1 - ss -tulpen | grep :80

This command shows what service is LISTENING on port 80:

    If you see nginx/apache → service is ready

    If no output → nothing is listening

Point 7: Command 2 - curl -I http://localhost

This command tests if web server responds:

    200 OK → web server working

    Connection refused → web server down

    Tests after handshake is complete

Point 8: Command 3 - nc -zv 127.0.0.1 80

This command tests if port 80 is OPEN:

    succeeded → port open and responding

    Connection refused → port closed

    timed out → packets dropped (firewall)

This actually performs the handshake test.
Point 9: Difference Between ss and nc

    ss shows who SHOULD be answering (passive info)

    nc actually TESTS if they DO answer (active test)

    ss can show a service running, but nc might still fail if service is hung

Point 10: Quick Error Reference
Error	What It Means
Connection refused	Server reached, port closed or service not running
Timeout	No response – packets dropped or route broken
DNS error	Name doesn't resolve – handshake never starts


Outcomes covered

RELATE HANDSHAKE TO APPLICATION BEHAVIOR
What Happens During Handshake vs What User Sees
Handshake Stage	What's Happening	What User Sees
SYN sent	Client trying to connect	"Connecting..."
SYN-ACK received	Server acknowledges	Still "Connecting..."
ACK sent	Connection established	Browser starts loading
Handshake fails	Something went wrong	Error message
Mapping Handshake Failures to User Errors
Handshake Problem	What User Sees in Browser	What It Means
Closed port	"Connection refused"	Service not running
Firewall drop	"Connection timed out"	Packets being blocked
Route issue	"Connection timed out"	Network path broken
Service not listening	"Connection refused"	Service crashed or not started
DNS error	"Server not found"	Can't find website address


PART 4: USE PORT TESTS TO VERIFY CONNECTIVITY
How Each Command Verifies Connectivity
Command	What It Tests	Handshake Stage	What Result Tells You
ss -tulpen | grep :80	Service availability	Before handshake	Is any service LISTENING?
nc -zv 127.0.0.1 80	Port reachability	SYN → SYN-ACK	Can handshake complete?
curl -I http://localhost	Application response	After handshake	Does app work correctly?





Practical lab

LAB TASK 1: Command 1 - ss -tulpen | grep :80

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day08$ ss -tulpen | grep :80
tcp   LISTEN 0      511          0.0.0.0:80         0.0.0.0:*    ino:11906 sk:a cgroup:/system.slice/nginx.service <->                                                                                                                                         
tcp   LISTEN 0      511             [::]:80            [::]:*    ino:11907 sk:e cgroup:/system.slice/nginx.service v6only:1 <->                                                                                                

LAB TASK 2: Command 2 - curl -I http://localhost

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day09$ curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 09 Mar 2026 22:07:14 GMT
Content-Type: text/html
Content-Length: 10671
Last-Modified: Wed, 19 Nov 2025 12:17:57 GMT
Connection: keep-alive
ETag: "691db575-29af"
Accept-Ranges: bytes


LAB TASK 3: Command 3 - nc -zv 127.0.0.1 80

nc -zv 127.0.0.1 80

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day09$ nc -zv 127.0.0.1 80
Connection to 127.0.0.1 80 port [tcp/http] succeeded!


LAB TASK 4: Test All 3 Commands Together

nc -zv 127.0.0.1 80
=== TASK 4: Complete Test ===

1. ss -tulpen | grep :80
tcp   LISTEN 0      511          0.0.0.0:80         0.0.0.0:*    ino:11906 sk:a cgroup:/system.slice/nginx.service <->                                                                                                                                         
tcp   LISTEN 0      511             [::]:80            [::]:*    ino:11907 sk:e cgroup:/system.slice/nginx.service v6only:1 <->                                                                                                                                

2. curl -I http://localhost
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0 10671    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 09 Mar 2026 22:10:36 GMT

3. nc -zv 127.0.0.1 80
Connection to 127.0.0.1 80 port [tcp/http] succeeded!


sudo systemctl stop nginx scenario

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day09$ ss -tulpen | grep :80
curl -I http://localhost
nc -zv 127.0.0.1 80
curl: (7) Failed to connect to localhost port 80 after 0 ms: Couldn't connect to server
nc: connect to 127.0.0.1 port 80 (tcp) failed: Connection refused

Command flags

DAY 9 - Command Flags
Command 1: ss -tulpen | grep :80
Flag	Full Form	What It Does
-t	TCP	Show TCP ports
-u	UDP	Show UDP ports
-l	Listening	Show only listening ports
-p	Process	Show which process is using the port
-e	Extended	Show extended information
-n	Numeric	Show port numbers (not service names)

In ss -tulpen: All flags together show you TCP and UDP ports that are Listening, with Process info, Extended details, and Numeric ports.
Command 2: curl -I http://localhost
Flag	Full Form	What It Does
-I	Head	Fetch only the HTTP headers, not the full page

Without -I: curl downloads the entire webpage (lots of output)
With -I: Only shows the response headers (quick test)



Command 3: nc -zv 127.0.0.1 80
Flag	Full Form	What It Does
-z	Zero I/O	Just test connection, don't send data
-v	Verbose	Show detailed output

Without -z: nc would try to send data and stay connected
With -z: Just checks if port is open, then disconnects

Without -v: No output (silent)
With -v: Shows "Connection succeeded" or error message


Hypothesis

Scenario 1

ss -tulpen | grep :80

tcp   LISTEN 0   511   0.0.0.0:80   users:(("nginx",pid=1234))

nc -zv 127.0.0.1 80

nc: connect to 127.0.0.1 port 80 (tcp) failed: Connection refused

curl -I http://localhost

curl: (7) Failed to connect to localhost port 80: Connection refused

My observation

Nginx is listening at 80 but not responding and transmitting data

Solution: systemctl restart nginx

AI response

✅ PERFECT ANSWER!

You've got it exactly right!
Your Hypothesis & Solution
Part	Your Answer	Correct?
Problem	"Nginx is listening at 80 but not responding and transmitting data"	✅ YES
Solution	systemctl restart nginx	✅ YES

SCENARIO 2

ss -tulpen | grep :80

(no output)


I think nginx or apache are not installed or they are disabled

 EXCELLENT ANSWER! You're Absolutely Right!
Your Hypothesis & Solution
Part	Your Answer	Correct?
Problem	"nginx or apache are not installed or they are disabled"	✅ YES
Solution	Install or start the service	✅ YES

What To Do Next

# Check if nginx is installed
which nginx

# Check if service exists
systemctl status nginx

# If not installed:
sudo apt update
sudo apt install -y nginx

# If installed but not running:
sudo systemctl start nginx
sudo systemctl enable nginx

Scenario 3 if timed out at nc and curl

Firewall issue solution sudo ufw allow 80/TDP



