10 points

1. DNS (Domain Name System) maps domain names (like google.com) to IP addresses (like 142.250.185.46). It's like a phonebook for the internet.

2.  DNS query flow: Client asks local resolver → resolver checks cache → if not found, queries upstream authoritative servers → resolver returns answer with TTL.

3.  Common DNS records:

   A record - maps hostname to IPv4 address

   AAAA record - maps hostname to IPv6 address

   CNAME record - alias (www.example.com to example.com)

   MX record - mail exchange server for domain

4. Beginner note: DNS is a phonebook, not the website itself. If DNS is wrong, the service may still be healthy but unreachable by name.

5. DHCP (Dynamic Host Configuration Protocol) provides automatic network configuration:

   IP address

   Subnet mask

   Default gateway

   DNS servers

   Lease duration

 6. DHCP lease lifecycle: Client requests IP → DHCP server offers → client accepts → server acknowledges with lease time → client renews before expiry.

 7. Beginner note: DHCP changes can break services if your app expects a fixed server IP.

8. Static IP means manually setting a fixed IP address instead of getting one from DHCP. Servers usually need stable addresses for predictable access and firewall rules.

9. Validation checklist after static IP change:

   ip a shows expected address

   ip route shows correct default gateway

   ping <gateway> works

   dig example.com works

10. Commands for today: resolvectl status, dig example.com A, nslookup example.com, ip a, ip route


Lab Task 1: resolvectl status - Check DNS Configuration

resolvectl status

Output:

l status
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (enp0s3)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 213.187.64.1
       DNS Servers: 217.69.224.73 213.187.64.1

Line	What it tells you
Link 2 (enp0s3)	Your network interface name is enp0s3 (similar to eth0)
Current DNS Server: 213.187.64.1	The DNS server your computer is currently using
DNS Servers: 217.69.224.73 213.187.64.1	Both DNS servers available

Lab Task 2: dig google.com A - Query DNS for IPv4 Address

dig google.com A

Output:

dig google.com A

; <<>> DiG 9.18.39-0ubuntu0.24.04.2-Ubuntu <<>> google.com A
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8216
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;google.com.			IN	A

;; ANSWER SECTION:
google.com.		120	IN	A	142.251.208.14 IPV 4 SECTION

;; Query time: 15 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Sun Mar 08 19:38:37 CET 2026
;; MSG SIZE  rcvd: 55

Lab Task 3: nslookup google.com - Alternative DNS Query

nslookup google.com

nslookup google.com
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	google.com
Address: 142.251.208.14
Name:	google.com
Address: 2a00:1450:4001:80c::200e

Google ip: 142.251.208.14


Lab Task 4: ip a - Check Current IP Address (Before Static IP)

ip a

: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:90:b6:d5 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 69161sec preferred_lft 69161sec
    inet6 fd17:625c:f037:2:5e1e:6d5d:b646:5295/64 scope global temporary dynamic 
       valid_lft 86183sec preferred_lft 14183sec
    inet6 fd17:625c:f037:2:a00:27ff:fe90:b6d5/64 scope global dynamic mngtmpaddr 
       valid_lft 86183sec preferred_lft 14183sec
    inet6 fe80::a00:27ff:fe90:b6d5/64 scope link 
       valid_lft forever preferred_lft forever


Lab Task 5: ip route - Check Default Gateway

Static ip 

STEP 1: FIND YOUR CURRENT NETWORK INFO

# Find your interface name
ip -4 -br a

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day07$ # Find your interface name
ip -4 -br a
lo               UNKNOWN        127.0.0.1/8 
enp0s3           UP             10.0.2.15/24 


# Find your gateway
ip route | grep default

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day07$ # Find your gateway
ip route | grep default
default via 10.0.2.2 dev enp0s3 proto dhcp src 10.0.2.15 metric 100

# Find your DNS servers
resolvectl status | grep "DNS Servers"

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day07$ # Find your DNS servers
resolvectl status | grep "DNS Servers"
       DNS Servers: 217.69.224.73 213.187.64.1


STEP 2: CHOOSE A STATIC IP

STEP 3: FIND NETPLAN FILE

ls /etc/netplan/

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day07$ ls /etc/netplan/
01-netcfg.yaml~  01-network-manager-all.yaml  50-cloud-init.yaml


STEP 4: BACKUP THE FILE (SAFE PRACTICE)

sudo cp /etc/netplan/00-installer-config.yaml /etc/netplan/00-installer-config.yaml.backup

STEP 5: EDIT THE FILE

sudo nano /etc/netplan/00-installer-config.yaml

STEP 6: APPLY THE STATIC IP

sudo netplan apply

(generate:8993): WARNING **: 21:44:07.223: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:8991): WARNING **: 21:44:09.616: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:8991): WARNING **: 21:44:10.130: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.


Validation 

# Check your new IP
ip a | grep inet

p a | grep inet
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host noprefixroute 
    inet 10.0.2.200/24 brd 10.0.2.255 scope global noprefixroute enp0s3
    inet 10.0.2.15/24 brd 10.0.2.255 scope global secondary dynamic noprefixroute enp0s3
    inet6 fd17:625c:f037:2:2e40:b6ae:b81f:3e32/64 scope global temporary dynamic 
    inet6 fd17:625c:f037:2:a00:27ff:fe90:b6d5/64 scope global dynamic mngtmpaddr 
    inet6 fe80::a00:27ff:fe90:b6d5/64 scope link 


ip route | grep default

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day07$ ip route | grep default
default via 10.0.2.2 dev enp0s3 proto static metric 100 
default via 10.0.2.2 dev enp0s3 proto dhcp src 10.0.2.15 metric 100 


# Check 3: ping gateway works
ping -c 2 10.0.2.2

$ # Check 3: ping gateway works
ping -c 2 10.0.2.2
PING 10.0.2.2 (10.0.2.2) 56(84) bytes of data.
64 bytes from 10.0.2.2: icmp_seq=1 ttl=255 time=0.239 ms
64 bytes from 10.0.2.2: icmp_seq=2 ttl=255 time=0.270 ms

--- 10.0.2.2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1047ms
rtt min/avg/max/mdev = 0.239/0.254/0.270/0.015 ms


dig google.com A +short

142.250.186.46

My understandings

WHY YOU CAN'T PING YOURSELF (USUALLY)
bash

# This is like calling your own phone number
ping 10.0.2.200

# Your computer receives the ping, but many systems block self-pings
# It's like: "Why are you calling yourself? Pick up the phone!"

Some systems will reply, some won't. But you're right - you're already there!

[YOUR VM] 10.0.2.200
     |
     |  "Are you there?"  ping 10.0.2.2
     ↓
[GATEWAY] 10.0.2.2
     |
     |  "Yes, I'm here!"  reply from 10.0.2.2
     ↑
[YOUR VM]

Hypothese:

SCENARIO 1: DNS Failure

Your Observation:

    ping 8.8.8.8 works (internet connectivity is fine)

    ping google.com fails with "Temporary failure in name resolution"

Your Hypothesis:
DNS problem - internet works but name resolution fails. The DNS servers might be wrong, unreachable, or misconfigured.

Your Hypothesis:
DNS problem - internet works but name resolution fails. The DNS servers might be wrong, unreachable, or misconfigured.

# Check current DNS configuration
resolvectl status

# Check DNS servers in resolv.conf
cat /etc/resolv.conf

# Test if DNS server is reachable
ping 217.69.224.73   # Your DNS server from earlier

# If DNS servers are wrong, update them
sudo nano /etc/netplan/01-network-manager-all.yaml

# Add correct DNS servers:
nameservers:
  addresses:
    - 8.8.8.8
    - 8.8.4.4

# Apply changes
sudo netplan apply

# Or temporarily use a working DNS
dig @8.8.8.8 google.com

Gateway Unreachable After Static IP

Your Observation:

    ip a shows IP 10.0.2.250/24 is set correctly

    ip route shows gateway 10.0.2.1 configured

    ping 10.0.2.1 fails with "Destination Host Unreachable"

Your Hypothesis:
Wrong gateway IP configured. In VirtualBox NAT network, gateway should be 10.0.2.2, not 10.0.2.1.

Commands to Test Hypothesis:

# Check what the correct gateway should be
# Before changing static IP, you had:
ip route | grep default
# Should show: default via 10.0.2.2

# Try pinging the correct gateway
ping 10.0.2.2


Day 7 Quiz
1. What does DNS resolve?
2. One common DNS record type for hostname to IPv4?
3. What does DHCP assign?
4. Why use static IP for servers?
5. Tool to query DNS manually?


My response

1. It finds IP address
2.A
3. Automatic IP with lease
4. Permanenet and certain
5. Nslookup







ip route

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day07$ ip route
default via 10.0.2.2 dev enp0s3 proto dhcp src 10.0.2.15 metric 100 
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100 

Command Flags

dig example.com A

Flag A  for specific record IPv4 address




My response

dig finds ip not domain I guess
You're Absolutely Right! I Need to Be More Precise

Let me correct myself:
WHAT dig ACTUALLY DOES
Command	What it does
dig google.com	Asks DNS: "What is the IP address for google.com?"
Result	Returns the IP address (142.250.185.46)
CORRECT STATEMENT

✅ dig finds the IP address FOR a domain name

❌ dig does NOT find domains (you already provide the domain)
YOUR UNDERSTANDING IS CORRECT
You type	You provide	dig gives you back
dig google.com	Domain: google.com	IP: 142.250.185.46
dig example.com	Domain: example.com	IP: 93.184.216.34
SO FOR QUIZ QUESTION 1

    "What does DNS resolve?"

Correct answer: DNS resolves domain names to IP addresses

Not the other way around!

Thanks for keeping me accurate!

