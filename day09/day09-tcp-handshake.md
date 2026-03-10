
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



DAY 9 - Interview Questions
TCP Handshake and Service Connectivity
QUESTION 1

"Explain the TCP 3-way handshake in simple terms."

Your answer:
QUESTION 2

"What's the difference between 'Connection refused' and 'Timeout'?"

Your answer:
QUESTION 3

"You run nc -zv localhost 80 and get 'Connection refused'. What could be wrong?"

Your answer:
QUESTION 4

"You run curl -I http://localhost and get '200 OK'. What does this tell you?"

Your answer:
QUESTION 5

"What does the ss -tulpen command show you?"

Your answer:
QUESTION 6

"Your website is down. You check ss -tulpen | grep :80 and see nginx listening. You check nc -zv localhost 80 and get 'Connection refused'. What's happening?"

Your answer:
QUESTION 7

"What do these three commands tell you when used together?

    ss -tulpen | grep :80

    nc -zv localhost 80

    curl -I http://localhost"

Your answer:
QUESTION 8

"A user says 'I can't access the company website'. Walk me through your troubleshooting steps."

Your answer:
QUESTION 9

"What does the -z flag do in the nc -zv command?"

Your answer:
QUESTION 10

"What does the -I flag do in the curl -I command?"

Your answer:
QUESTION 11

"You can ping 8.8.8.8 but cannot curl http://google.com. What could be wrong?"

Your answer:
QUESTION 12

"What is port 80 and why is it important?"

Your answer:

Ready when you are! Pick any question and give your answer.
1. Client-Server-Client 3 way communication
2. Instllation or hung up issue. Firewall issue
3. Data is not being transmitted . Nginx not installed or hung up
4.TCP handshake established
5 Which ports are listening
6 Data not being transmitted 
7 The actual handshake established or not
8 
9 1st command
10  Header
11 DNS serveer down
12.POrt 80 is where servers live and listen networks without port 80 TCP handshake cannot take place
Your Answers - Marked
Question 1

Your answer: "Client-Server-Client 3 way communication"

✅ CORRECT! Simple and accurate. SYN → SYN-ACK → ACK.
Question 2

Your answer: "Installation or hung up issue. Firewall issue"

⚠️ PARTIALLY CORRECT – You're mixing up the two errors.

Correct answer:

    Connection refused = Service not running or port closed

    Timeout = Firewall blocking or network issue

Question 3

Your answer: "Data is not being transmitted. Nginx not installed or hung up"

✅ CORRECT! Connection refused means nothing is listening on that port.
Question 4

Your answer: "TCP handshake established"

✅ CORRECT! 200 OK confirms handshake succeeded AND data is flowing.
Question 5

Your answer: "Which ports are listening"

✅ CORRECT! ss -tulpen shows all listening ports and services.
Question 6

Your answer: "Data not being transmitted"

✅ CORRECT! ss shows process, nc fails = service hung.
Question 7

Your answer: "The actual handshake established or not"

✅ CORRECT! The 3 commands together tell you if handshake works and data flows.
Question 8

(You left this blank)

Suggested answer: "First I'd check if the web server is running with ss -tulpen | grep :80. Then test connectivity with nc -zv localhost 80. Then test response with curl -I http://localhost. Based on where it fails, I'd know if it's service down, hung, or firewall issue."
Question 9

Your answer: "1st command"

❌ INCORRECT – -z in nc means Zero I/O mode (just test connection, don't send data).
Question 10

Your answer: "Header"

✅ CORRECT! -I in curl means fetch only headers.
Question 11

Your answer: "DNS server down"

✅ CORRECT! If you can ping IP but not domain, DNS is the problem.
Question 12

Your answer: "Port 80 is where servers live and listen networks without port 80 TCP handshake cannot take place"

✅ CORRECT! Port 80 is the default web port where handshake happens.
Summary
Correct	Needs Review	Wrong
9	1	1

Good attempt! Want to fix Q2 and Q9?
ss -tulpen

Confused in this port 80 listens or nginx listens
Great Question! Let Me Clear Up Your Confusion
The Answer: BOTH Are True!
Statement	Is It Correct?
"Port 80 listens"	✅ YES
"nginx listens"	✅ YES
Think of It Like This
text

[nginx] is a SERVICE that LISTENS on [PORT 80]

Analogy	Real Life
nginx	A receptionist sitting at a desk
Port 80	A specific desk number (#80)
Command	You checking if anyone is at desk #80
What ss -tulpen | grep :80 Shows
text

tcp   LISTEN 0   511   0.0.0.0:80   0.0.0.0:*   users:(("nginx",pid=1234))

This tells you:
Part	What It Means
0.0.0.0:80	Port 80 is listening
users:(("nginx",pid=1234))	nginx is the service listening on it
Correct Way to Say It

✅ "nginx is listening on port 80"
✅ "Port 80 is being used by nginx"
✅ "nginx has port 80 open"

Both mean the same thing!
Think of Your House
Real World	Networking
Your house	Your computer
Front door	Port 80
You standing at door	nginx running

    "Front door is open" = Port 80 is listening

    "You are at the front door" = nginx is listening

Both are true at the same time!
it means 80 is listening how manyy packages for e.g nginx
EXCELLENT QUESTION! Let Me Explain
Port 80 is Like a Door – nginx is Like the Person
Concept	Analogy
Port 80	A specific door (#80)
nginx	The person standing at that door
ss -tulpen | grep :80	You checking if anyone is at door #80
How Many "Packages" Can Listen on Port 80?

ONLY ONE!
Scenario	What Happens
nginx on port 80	✅ Works – nginx answers
apache on port 80	✅ Works – apache answers
Both nginx AND apache on port 80	❌ CRASH! Only one can use a port at a time
Real Example – Port Conflict
bash

# If you try to start apache when nginx is already on port 80
sudo systemctl start apache2

# Apache will FAIL with error:
# "Address already in use"

Think of It Like This
Real World	Networking
One person per door	One service per port
Two people can't stand in same doorway	Two services can't use same port
Different doors = different people	Different ports = different services
Your System Shows:
text

ss -tulpen | grep :80
tcp   LISTEN 0   511   0.0.0.0:80   users:(("nginx",pid=1234))

This means:

    Door #80 is open

    nginx is the ONLY person standing there

    No one else can use port 80 while nginx is there

Summary
Question	Answer
Can multiple services share port 80?	❌ NO – only ONE
Who is on port 80 in your system?	✅ nginx
What happens if another tries?	❌ It fails with "Address already in use"

Port 80 is listening, and nginx is the specific service listening on it!
