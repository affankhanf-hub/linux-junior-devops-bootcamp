## Environment Note

**This lab was completed in WSL (Windows Subsystem for Linux).** WSL shares the Windows network stack. My current WSL session shows:

| Setting | Current Value |
|---------|---------------|
| Interface | eth0 |
| IP Address | 172.25.169.7/20 |
| Gateway | 172.25.160.1 |
| DNS Server | 172.25.160.1 |

## Theory Summary

1. **DNS (Domain Name System)** maps domain names (like google.com) to IP addresses (like 142.250.185.46). It is like a phonebook for the internet.

2. **DNS query flow:** Client asks local resolver → resolver checks cache → if not found, queries upstream authoritative servers → resolver returns answer with TTL.

3. **Common DNS records:**
   - A record - maps hostname to IPv4 address
   - AAAA record - maps hostname to IPv6 address
   - CNAME record - alias (www.example.com to example.com)
   - MX record - mail exchange server for domain

4. **Beginner note:** DNS is a phonebook, not the website itself. If DNS is wrong, the service may still be healthy but unreachable by name.

5. **DHCP (Dynamic Host Configuration Protocol)** provides automatic network configuration:
   - IP address
   - Subnet mask
   - Default gateway
   - DNS servers
   - Lease duration

6. **DHCP lease lifecycle:** Client requests IP → DHCP server offers → client accepts → server acknowledges with lease time → client renews before expiry.

7. **Beginner note:** DHCP changes can break services if your app expects a fixed server IP.

8. **Static IP** means manually setting a fixed IP address instead of getting one from DHCP. Servers usually need stable addresses for predictable access and firewall rules.

9. **Validation checklist after static IP change:**
   - `ip a` shows expected address
   - `ip route` shows correct default gateway
   - `ping <gateway>` works
   - `dig example.com` works

10. **Commands for today:** `resolvectl status`, `dig`, `nslookup`, `ip a`, `ip route`

## Commands Used

| Command | Purpose |
|---------|---------|
| `ip a` | Show all IP addresses assigned to network interfaces |
| `ip route` | Show routing table and default gateway |
| `resolvectl status` | Show current DNS configuration |
| `ping <IP>` | Test connectivity to an IP address |
| `dig google.com +short` | Query DNS for Google's IP address |

## Output / Evidence

### 1. ip a - Show IP Address

```bash
$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:08:96:a4 brd ff:ff:ff:ff:ff:ff
    inet 172.25.169.7/20 brd 172.25.175.255 scope global eth0
What this shows: My interface is eth0 and my current IP address is 172.25.169.7.

2. ip route - Show Gateway

bash
$ ip route
default via 172.25.160.1 dev eth0 proto kernel
172.25.160.0/20 dev eth0 proto kernel scope link src 172.25.169.7
What this shows: My default gateway is 172.25.160.1.

3. resolvectl status - Show DNS Configuration
bash

$ resolvectl status
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: foreign
Current DNS Server: 172.25.160.1
       DNS Servers: 172.25.160.1

Link 2 (eth0)
    Current Scopes: none
         Protocols: -DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
What this shows: My DNS server is 172.25.160.1 (same as my gateway).

4. ping - Test Connectivity
bash

$ ping 172.25.169.7
PING 172.25.169.7 (172.25.169.7) 56(84) bytes of data.
64 bytes from 172.25.169.7: icmp_seq=1 ttl=64 time=0.041 ms
64 bytes from 172.25.169.7: icmp_seq=2 ttl=64 time=0.078 ms
64 bytes from 172.25.169.7: icmp_seq=3 ttl=64 time=0.080 ms
--- 172.25.169.7 ping statistics ---
13 packets transmitted, 13 received, 0% packet loss
What this shows: My network interface is working correctly. I can ping my own IP address.

5. dig - Test DNS Resolution
bash

$ dig google.com +short
142.251.110.102
142.251.110.138
142.251.110.100
142.251.110.113
142.251.110.139
142.251.110.101

What this shows: DNS resolution works. Google.com resolves to multiple IP addresses.

Validation

Requirement	Command	My Result	Status
Show IP address	ip a	172.25.169.7	✅ PASS
Show gateway	ip route	172.25.160.1	✅ PASS
Show DNS config	resolvectl status	172.25.160.1	✅ PASS
Ping works	ping 172.25.169.7	0% loss	✅ PASS
DNS resolution	dig google.com +short	6 IPs returned	✅ PASS
Troubleshooting
Issue 1: dig command not found
bash

$ dig google.com
Command 'dig' not found, but can be installed with:
sudo apt install bind9-dnsutils
Root cause: The dig command is not installed by default on minimal WSL.

Resolution: Installed bind9-dnsutils package.

bash
sudo apt update
sudo apt install bind9-dnsutils -y
Issue 2: WSL Cannot Set Static IP
Root cause: WSL shares the Windows network stack. The IP address is assigned by Windows DHCP, not by Linux netplan.

Resolution: For this lab, I documented the WSL limitation and focused on understanding DNS, DHCP concepts, and running the observation commands.

Summary of My Current Network Configuration
Setting	Value
Interface	eth0
My IP Address	172.25.169.7
Subnet Mask	/20 (255.255.240.0)
Gateway	172.25.160.1
DNS Server	172.25.160.1
DNS Resolution	Working (google.com → 142.251.110.102)
Command Flags Reference
Command	Flag	What it does
dig	+short	Shows only the IP address, no extra output
ping	-c 4	Sends exactly 4 packets (default runs forever)



Quiz Answers

Question	My Answer
1. What does DNS resolve?	Domain names to IP addresses
2. One common DNS record type for hostname to IPv4?	A record
3. What does DHCP assign?	Automatic IP address with lease
4. Why use static IP for servers?	Permanent and certain - servers need predictable addresses
5. Tool to query DNS manually?	nslookup or dig

Lessons Learned

DNS is a phonebook - it translates domain names (google.com) to IP addresses (142.251.110.102). Without DNS, you can still reach websites by typing their IP address directly.

DHCP is automatic - it gives your computer an IP, gateway, and DNS servers without you doing anything. This is good for laptops but not for servers.

Static IP is manual - servers need static IP so other devices can always find them. You cannot set a true static IP in WSL because Windows controls the network.

The 5 validation commands work together: ip a (your IP), ip route (your gateway), resolvectl status (your DNS), ping (connectivity), dig (DNS resolution).

dig +short is cleaner than plain dig - it shows only the IP address without all the extra output.

WSL has limitations for network labs. For static IP practice, use VirtualBox. For DNS and DHCP observation, WSL works fine.

My current WSL network: IP 172.25.169.7, Gateway 172.25.160.1, DNS 172.25.160.1

