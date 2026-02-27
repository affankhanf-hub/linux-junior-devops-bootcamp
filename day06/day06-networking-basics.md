1.IP address (Your device name) When you're online, your phone/laptop/computer needs a unique identifier. That's your IP address
2.192.168.1.20 192.168.1 → Identifies your network (which neighborhood you live in)
   20 → Identifies YOU (your specific device)
3.Subnet mask: splits IP address into 2 parts
4.Subnet identifies 255 as network and 0 as device.
5.Subnet tells whether device is in the neighborhood or outside the boundary
6.Default gateway is the exit door 
7.If your IP is 10.0.0.50, subnet mask is 255.255.255.0, and gateway is 10.0.0.1...

   What's your network? → 10.0.0
   Can you talk directly to 10.0.0.99? → Yes, same network
   Can you talk directly to 10.0.1.5? → No, different network (needs gateway) 
8./24 tells you both subnet mask and network
9.

Practical lab

IP A
ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:90:b6:d5 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 51618sec preferred_lft 51618sec
    inet6 fd17:625c:f037:2:2f79:9dfc:e6ee:ee62/64 scope global temporary dynamic 
       valid_lft 86130sec preferred_lft 14130sec
    inet6 fd17:625c:f037:2:a00:27ff:fe90:b6d5/64 scope global dynamic mngtmpaddr 
       valid_lft 86130sec preferred_lft 14130sec
    inet6 fe80::a00:27ff:fe90:b6d5/64 scope link 
       valid_lft forever preferred_lft forever


Your IP	10.0.2.15
Subnet mask	255.255.255.0 (because /24)
Your network	10.0.2.0
Broadcast	10.0.2.255
Available IPs	10.0.2.1 to 10.0.2.254


IP route

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ ip route
default via 10.0.2.2 dev enp0s3 proto dhcp src 10.0.2.15 metric 100 
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100

ping -c 4 8.8.8.8 - Test Internet Connectivity

ping -c 4 8.8.8.8

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ ping -c 4 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=255 time=18.9 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=255 time=14.8 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=255 time=13.2 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=255 time=12.6 ms

--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3007ms
rtt min/avg/max/mdev = 12.641/14.880/18.894/2.455 ms


traceroute 8.8.8.8 - Trace Path to Destination

traceroute 8.8.8.8

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  _gateway (10.0.2.2)  4.355 ms  4.320 ms  4.292 ms
 2  * * *
 3  * * *
 4  * * *
 5  * * *
 6  * * *
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * * *
25  * * *
26  * * *
27  * * *
28  * * *
29  * * *
30  * * *

As it is VM environment


Flags

Hypothesis

Scenario: Imagine I cannot reach the internet

HYPOTHESE
Based on what I see, I think:

    Network cable might be unplugged

    WiFi might be disconnected

    DHCP might have failed

Check cable whether it is unplugged

ip a        # Should now show IP address
ip route    # Should show default gateway
ping -c 4 8.8.8.8   # Should work
traceroute 8.8.8.8  # Should show path

Flags

ip -4 a           # Show only IPv4
ip -6 a           # Show only IPv6
ip -br a          # Brief output (cleaner)
ip -br -4 a       # Brief IPv4 only

ip route show     # Same as ip route
ip route list     # Same as ip route
ip route get 8.8.8.8  # Show route for specific destination
ip -4 route       # Show only IPv4 routes

ping -c 4 8.8.8.8     # Count (send 4 packets)
ping -i 2 8.8.8.8     # Interval (2 seconds between packets)
ping -W 3 8.8.8.8     # Timeout (wait 3 seconds for reply)
ping -s 100 8.8.8.8   # Packet size (100 bytes)
ping -q 8.8.8.8       # Quiet (only summary)
ping -4 8.8.8.8       # Force IPv4
ping -6 8.8.8.8       # Force IPv6

traceroute -n 8.8.8.8      # No DNS resolution (faster)
traceroute -m 15 8.8.8.8   # Max 15 hops
traceroute -w 3 8.8.8.8    # Wait 3 seconds for reply
traceroute -q 2 8.8.8.8    # 2 probes per hop
traceroute -4 8.8.8.8      # Force IPv4
traceroute -6 8.8.8.8      # Force IPv6
traceroute -I 8.8.8.8      # Use ICMP instead of UDP


Interview Questions - Daily Micro Drill

If the host can ping the gateway but not the internet, I suspect an upstream routing, NAT, or firewall issue. This means:

    Local network is working (gateway is reachable)

    The problem is between the router and the internet

I would check:

    Is the router's internet light on?

    Can the router itself ping 8.8.8.8? (log into router admin)

    Is NAT configured correctly on the router?

    Is the ISP having an outage?

    Is there a firewall on the router blocking outbound traffic?

The fix is not on the user's computer - it's on the IP side


Question 2: "A user can ping 8.8.8.8 but cannot ping google.com. What commands would you use to diagnose this?"

My answer:
"If they can ping 8.8.8.8 (an IP) but cannot ping google.com (a domain), this is a DNS problem. They have internet connectivity but name resolution fails.

I would use these commands from the troubleshooting ladder:
Command	What it checks
dig google.com	Tests DNS resolution directly
nslookup google.com	Alternative DNS test
cat /etc/resolv.conf	Shows configured DNS servers
resolvectl status	Shows current DNS configuration

If dig google.com fails but ping 8.8.8.8 works, DNS is definitely the problem."


Question 3: "Walk me through the complete 5-step troubleshooting ladder from the textbook."

My answer:
"I follow this exact 5-step ladder from Chapter 7.7.4:
Step	Command	What it checks
1	ip a	Check interface IP address
2	ip route	Check route/default gateway
3	ping <gateway> then ping 8.8.8.8	Check raw connectivity
4	dig example.com	Check name resolution (DNS)
5	sudo ufw status verbose	Check local firewall state

Question 4: "What is NAT and how does it relate to the 'ping gateway works but internet fails' scenario?"

My answer:
"NAT stands for Network Address Translation. It's how my router takes my private IP (192.168.1.20) and translates it to a public IP so I can access the internet.

In the 'ping gateway works but internet fails' scenario:

    I can reach the router (gateway ping works)

    But when the router tries to send my traffic to the internet, NAT might be broken

Without working NAT, my private IP cannot communicate with public internet servers. The router receives my packets but cannot translate and forward them. This is why we suspect NAT along with upstream routing and firewall issues."



Quiz

What does subnet mask represent?
2. What is default gateway?
3. Command to show routing table?
4. Difference between TCP and UDP (basic)?
5. Why can ping fail while DNS works?


I will answer do not give answers

My answers

Subnet mask tells which part is the network and which is the host
Default gateway or router  is the point through which ping goes to connect with IP 
IP route
TCP is slow but corrects data UDP is fast but ignores corrections
Internet connnectivity issue, up stream routing or firewall

AI response

Feedback on Your Answers:

Q1: What does subnet mask represent?
Your answer: "Subnet mask tells which part is the network and which is the host"

    ✅ CORRECT! Simple and accurate. The subnet mask separates the network portion from the host portion.

Q2: What is default gateway?
Your answer: "Default gateway or router is the point through which ping goes to connect with IP"

    ✅ BASICALLY CORRECT but could be clearer. The default gateway is the router that connects your local network to other networks (like the internet). All traffic destined for outside your network goes through it.

Q3: Command to show routing table?
Your answer: "IP route"

    ✅ CORRECT! The command is ip route (or ip route show).

Q4: Difference between TCP and UDP (basic)?
Your answer: "TCP is slow but corrects data UDP is fast but ignores corrections"

    ✅ GOOD SIMPLE ANSWER! TCP ensures data arrives correctly (retransmits if lost), UDP sends and forgets. TCP = reliable but slower, UDP = faster but no guarantees.

Q5: Why can ping fail while DNS works?
Your answer: "Internet connectivity issue, up stream routing or firewall"

    ⚠️ THIS ONE NEEDS WORK. The question asks "Why can ping fail while DNS works?" but your answer describes the opposite scenario.

The question is asking: If ping to an IP fails, but DNS still works, why?

Think about it: If DNS works, that means dig google.com returns an IP address. But if ping to that IP fails, what could be wrong?


My resposne

NAT 

AI response

Simple Correct Answer:

"Ping can fail while DNS works because:

    DNS is just a lookup service - it returns an IP address

    But the destination server might be down, blocking ping, or a firewall might be blocking ICMP traffic while allowing DNS"

My response

why the fuck you did not teach me this


