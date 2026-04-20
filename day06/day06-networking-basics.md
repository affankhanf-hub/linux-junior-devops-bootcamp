##  Theory Summary

1. IP address is a **network address** - a unique numerical identifier for your device on a network. Every phone, laptop, or computer needs one when online.

2. 192.168.1.20
   - 192.168.1 → Identifies your network (which neighborhood you live in)
   - 20 → Identifies YOU (your specific device)

3. Subnet mask is a 32-bit number that separates an IP address into the **network portion** and the **host portion** using binary AND operations.

4. In a `/24` subnet (`255.255.255.0`), the first three octets (255.255.255) represent the network, and the last octet (0) represents the host.

5. Subnet tells whether a device is in your local network or outside the boundary. Devices in the same subnet can communicate directly without a router.

6. Default gateway is the router interface IP address that forwards traffic from your local network to destinations outside it. All non-local traffic goes through this "exit door."

7. If your IP is 10.0.0.50, subnet mask is 255.255.255.0, and gateway is 10.0.0.1...
   - What's your network? → 10.0.0
   - Can you talk directly to 10.0.0.99? → Yes, same network
   - Can you talk directly to 10.0.1.5? → No, different network (needs gateway)

8. `/24` tells you both subnet mask (255.255.255.0) and that the first 24 bits belong to the network portion.

9. A **route** is an instruction in the system's routing table that tells the kernel which interface and next-hop address to use for a given destination network.

---

## Commands Used

| # | Command | Purpose |
|---|---------|---------|
| 1 | `ip a` | Show all network interfaces and IP addresses |
| 2 | `ip route` | Display routing table |
| 3 | `ping -c 4 8.8.8.8` | Test internet connectivity (4 packets) |
| 4 | `traceroute 8.8.8.8` | Trace path to destination |
| 5 | `ip -br -4 a` | Brief IPv4 only output |

---

Output & Evidence

### Practical Lab - ip a

```bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ ip a
text
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
My Analysis
Field	Value
Your IP	10.0.2.15
Subnet mask	255.255.255.0 (because /24)
Your network	10.0.2.0
Broadcast	10.0.2.255
Available IPs	10.0.2.1 to 10.0.2.254
IP Route
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ ip route
text
default via 10.0.2.2 dev enp0s3 proto dhcp src 10.0.2.15 metric 100 
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
What these routes mean:

default via 10.0.2.2 - Any traffic not destined for my local network goes to gateway 10.0.2.2

10.0.2.0/24 dev enp0s3 - Traffic to my own subnet is sent directly (no gateway needed)

Ping - Test Internet Connectivity
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ ping -c 4 8.8.8.8
text
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=255 time=18.9 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=255 time=14.8 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=255 time=13.2 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=255 time=12.6 ms

--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3007ms
rtt min/avg/max/mdev = 12.641/14.880/18.894/2.455 ms
What this proves: Internet connectivity is working with 0% packet loss. Average round trip time is 14.88 ms.

Traceroute - Trace Path to Destination
bash
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ traceroute 8.8.8.8
text
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  _gateway (10.0.2.2)  4.355 ms  4.320 ms  4.292 ms
 2  * * *
 3  * * *
 4  * * *
 5  * * *
... (all remaining hops show * * * to hop 30)
Note: This is a VM environment. Hops beyond the gateway show * * * because VirtualBox NAT blocks ICMP responses. On physical hardware, traceroute would show each router along the path.

Flags Reference
IP Command Flags
Command	What it does
ip -4 a	Show only IPv4
ip -6 a	Show only IPv6
ip -br a	Brief output (cleaner)
ip -br -4 a	Brief IPv4 only
ip route show	Same as ip route
ip route list	Same as ip route
ip route get 8.8.8.8	Show route for specific destination
ip -4 route	Show only IPv4 routes
Ping Command Flags
Command	What it does
ping -c 4 8.8.8.8	Count (send 4 packets)
ping -i 2 8.8.8.8	Interval (2 seconds between packets)
ping -W 3 8.8.8.8	Timeout (wait 3 seconds for reply)
ping -s 100 8.8.8.8	Packet size (100 bytes)
ping -q 8.8.8.8	Quiet (only summary)
ping -4 8.8.8.8	Force IPv4
ping -6 8.8.8.8	Force IPv6
Traceroute Command Flags
Command	What it does
traceroute -n 8.8.8.8	No DNS resolution (faster)
traceroute -m 15 8.8.8.8	Max 15 hops
traceroute -w 3 8.8.8.8	Wait 3 seconds for reply
traceroute -q 2 8.8.8.8	2 probes per hop
traceroute -4 8.8.8.8	Force IPv4
traceroute -6 8.8.8.8	Force IPv6
traceroute -I 8.8.8.8	Use ICMP instead of UDP

Hypothesis
Scenario: Imagine I cannot reach the internet

My Hypothesis:
Based on what I see, I think:

Network cable might be unplugged (or VM network adapter disabled)

WiFi might be disconnected (or VM NAT configuration incorrect)

DHCP might have failed

Check:

bash
ip a        # Should now show IP address (look for state UP and an assigned IP)
ip route    # Should show default gateway
ping -c 4 8.8.8.8   # Should work
traceroute 8.8.8.8  # Should show path
🪜 Troubleshooting Ladder
This is my 5-step troubleshooting ladder for network issues:

Step	Command	What it checks	If this fails...
1	ip a	Does my interface have an IP address?	DHCP failed or interface down
2	ip route	Do I have a default gateway?	No route to outside networks
3	ping <gateway> then ping 8.8.8.8	Can I reach my router? Can I reach the internet?	Gateway down or internet down
4	dig google.com	Does DNS resolution work?	DNS server misconfigured
5	sudo ufw status verbose	Is my local firewall blocking traffic?	Firewall rule too strict
Key principle: Follow in order. Do not skip to step 4 if step 1 fails.

Interview Questions - Daily Micro Drill
Question 1: "If the host can ping the gateway but not the internet, what do you suspect?"
My answer:

I suspect an upstream routing, NAT, or firewall issue. This means:

Local network is working (gateway is reachable)

The problem is between the router and the internet

What this actually means technically:

Ping to gateway succeeds → Layer 2 (Ethernet) and Layer 3 (IP) to local router are working

Ping to 8.8.8.8 fails → Either the router cannot forward traffic to the internet, or return traffic cannot reach you

I would check:

Is the router's internet light on?

Can the router itself ping 8.8.8.8? (log into router admin)

Is NAT configured correctly on the router?

Is the ISP having an outage?

Is there a firewall on the router blocking outbound traffic?

The fix is not on the user's computer - it's on the ISP side.

Question 2: "A user can ping 8.8.8.8 but cannot ping google.com. What commands would you use to diagnose this?"
My answer:

If they can ping 8.8.8.8 (an IP) but cannot ping google.com (a domain), this is a DNS problem. They have internet connectivity but name resolution fails.

I would use these commands from the troubleshooting ladder:

Command	What it checks
dig google.com	Tests DNS resolution directly
nslookup google.com	Alternative DNS test
cat /etc/resolv.conf	Shows configured DNS servers
resolvectl status	Shows current DNS configuration
If dig google.com fails but ping 8.8.8.8 works, DNS is definitely the problem.

Question 3: "Walk me through the complete 5-step troubleshooting ladder from the textbook."
My answer:

I follow this exact 5-step ladder:

Step	Command	What it checks
1	ip a	Check interface IP address
2	ip route	Check route/default gateway
3	ping <gateway> then ping 8.8.8.8	Check raw connectivity
4	dig example.com	Check name resolution (DNS)
5	sudo ufw status verbose	Check local firewall state
Question 4: "What is NAT and how does it relate to the 'ping gateway works but internet fails' scenario?"
My answer:

NAT stands for Network Address Translation. It's how my router takes my private IP (192.168.1.20) and translates it to a public IP so I can access the internet.

In the 'ping gateway works but internet fails' scenario:

I can reach the router (gateway ping works)

But when the router tries to send my traffic to the internet, NAT might be broken

Without working NAT, my private IP cannot communicate with public internet servers. The router receives my packets but cannot translate and forward them. This is why we suspect NAT along with upstream routing and firewall issues.

Quiz
#	Question	My Answer
1	What does subnet mask represent?	Subnet mask tells which part is the network and which is the host
2	What is default gateway?	Default gateway is the router interface that connects your local network to other networks like the internet
3	Command to show routing table?	ip route
4	Difference between TCP and UDP (basic)?	TCP is reliable but slower (retransmits lost data). UDP is faster but gives no guarantees.
5	Why can ping fail while DNS works?	DNS is just a lookup service that returns IP addresses. Ping failing while DNS works means the destination server might be down, blocking ICMP traffic, or a firewall is blocking ping but allowing DNS traffic.
Maintenance Checklist (Networking)

What was examined	Why it matters	Status	What should be reviewed
ip a output	Shows if interface has IP address and is UP	✅ Working (10.0.2.15, state UP)	Review if IP changes after reboot
ip route output	Shows default gateway exists	✅ Working (10.0.2.2)	Review if gateway changes
ping 8.8.8.8	Tests internet connectivity	✅ Working (0% loss, 14.88ms avg)	Monitor for packet loss over time
traceroute	Shows path to destination	Limited visibility in VM	Test on physical hardware or cloud VM for full path

Security Relevance
Topic	Security Impact
IP address exposure	Private IPs (like 10.0.2.15) are safe behind NAT. Public IPs are direct attack targets.
Default gateway	If a gateway is compromised, all traffic can be intercepted (man-in-the-middle attack).
DNS resolution	DNS spoofing can redirect traffic to malicious sites. Always verify DNS servers with resolvectl status.
ICMP (ping)	Often blocked by firewalls to prevent network reconnaissance. Ping failing does not always mean a host is down.
traceroute	Reveals network topology. Security teams may block ICMP to hide internal network structure.
Regular updates	Network tools and kernel updates often include security patches for network protocols.

Lessons Learned
IP address is a numerical network address - Not a "device name." Hostnames are names; IP addresses are numbers.

Subnet mask uses binary AND - It doesn't just "split" the address. The mask 255.255.255.0 in binary is 24 ones followed by 8 zeros.

Default gateway is a specific IP address - Not just an "exit door." It is the router's interface IP.

Routing table has two types of entries:

Direct routes (same subnet) - no gateway needed

Default route (everything else) - must use gateway

VM environments behave differently from physical hardware - My traceroute showed * * * after hop 1 because VirtualBox NAT blocks ICMP. On real hardware, I would see all hops.

DNS working does not mean internet is fully working - DNS is just a lookup service. If ping to an IP fails but DNS works, the destination is unreachable (down, firewall, or routing issue).

Follow the troubleshooting ladder in order - Do not check DNS if ip a shows no IP address. Fix layer by layer from bottom to top.

