# Part 8: Networking

[← Back to Part 7](./07-system-administration.md) | [Main Guide](../README.md) | [Quick Reference →](./quick-reference.md)

---

## Table of Contents

- [Network Basics](#network-basics)
- [Network Configuration](#network-configuration)
- [File Sharing](#file-sharing)
- [Network Troubleshooting](#network-troubleshooting)
- [DNS](#dns)

---

## Network Basics

### Network Components

- **ISP** - Internet Service Provider
- **Router** - Connects network to internet
- **WAN** - Wide Area Network (internet)
- **LAN** - Local Area Network (wired)
- **WLAN** - Wireless Local Area Network
- **Host** - Device on network

### TCP/IP Model Layers

**Application Layer:**

- HTTP, HTTPS - Web
- SMTP - Email
- FTP - File transfer
- DNS - Name resolution
- SSH - Secure shell

**Transport Layer:**

- TCP - Reliable delivery
- UDP - Fast, unreliable delivery

**Network Layer:**

- IP - Routing
- ICMP - Error messages

**Link Layer:**

- Ethernet
- WiFi

---

### Network Addressing

**MAC Address:**  
Physical address (48-bit)

```
00:1A:2B:3C:4D:5E
```

**IPv4 Address:**  
Logical address (32-bit)

```
192.168.1.100
```

**IPv6 Address:**  
Modern addressing (128-bit)

```
2001:0db8:85a3::8a2e:0370:7334
```

**Ports:**  
Application endpoints (0-65535)

- 22: SSH
- 80: HTTP
- 443: HTTPS
- 3306: MySQL
- 5432: PostgreSQL

---

### Subnets

**Subnet mask:**  
Defines network and host portions

**Common masks:**

- 255.255.255.0 (/24) - 254 hosts
- 255.255.0.0 (/16) - 65,534 hosts
- 255.255.255.128 (/25) - 126 hosts

**CIDR notation:**

```
192.168.1.0/24
```

---

## Network Configuration

### ifconfig (Legacy)

**View all interfaces:**

```bash
ifconfig
```

**Specific interface:**

```bash
ifconfig eth0
```

**Configure interface:**

```bash
sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0 up
```

**Bring interface up/down:**

```bash
sudo ifup eth0
sudo ifdown eth0
```

---

### ip (Modern)

**View interfaces:**

```bash
ip link show
```

**View IP addresses:**

```bash
ip addr show
```

**Specific interface:**

```bash
ip addr show eth0
```

**Add IP address:**

```bash
sudo ip addr add 192.168.1.100/24 dev eth0
```

**Remove IP address:**

```bash
sudo ip addr del 192.168.1.100/24 dev eth0
```

**Bring interface up/down:**

```bash
sudo ip link set eth0 up
sudo ip link set eth0 down
```

**View statistics:**

```bash
ip -s link show eth0
```

---

### Routing

**View routing table:**

```bash
route -n
```

**Or with ip:**

```bash
ip route show
```

**Add route:**

```bash
sudo route add -net 192.168.2.0/24 gw 10.0.0.1
```

**Or with ip:**

```bash
sudo ip route add 192.168.2.0/24 via 10.0.0.1
```

**Delete route:**

```bash
sudo route del -net 192.168.2.0/24
```

**Or with ip:**

```bash
sudo ip route del 192.168.2.0/24
```

**Default gateway:**

```bash
sudo ip route add default via 192.168.1.1
```

---

### DHCP

**Request new IP:**

```bash
sudo dhclient
```

**Release IP:**

```bash
sudo dhclient -r
```

**Specific interface:**

```bash
sudo dhclient eth0
```

---

### arp - Address Resolution

**View ARP cache:**

```bash
arp
```

**Or with ip:**

```bash
ip neighbour show
```

**Add ARP entry:**

```bash
sudo arp -s 192.168.1.100 00:1A:2B:3C:4D:5E
```

**Delete ARP entry:**

```bash
sudo arp -d 192.168.1.100
```

---

## File Sharing

### scp - Secure Copy

**Copy to remote:**

```bash
scp myfile.txt user@remote:/path/
```

**Copy from remote:**

```bash
scp user@remote:/path/file.txt ./
```

**Copy directory:**

```bash
scp -r mydir/ user@remote:/path/
```

**Specify port:**

```bash
scp -P 2222 file.txt user@remote:/path/
```

---

### rsync - Efficient Sync

**Basic sync:**

```bash
rsync -avz source/ destination/
```

**Common options:**

- `-a` - Archive mode
- `-v` - Verbose
- `-z` - Compress
- `-r` - Recursive
- `-h` - Human-readable

**To remote:**

```bash
rsync -avz /local/dir/ user@remote:/remote/dir/
```

**From remote:**

```bash
rsync -avz user@remote:/remote/dir/ /local/dir/
```

**Dry run:**

```bash
rsync -avzn source/ dest/
```

**Show progress:**

```bash
rsync -avz --progress source/ dest/
```

---

### Simple HTTP Server

**Python 3:**

```bash
python3 -m http.server 8000
```

**Python 2:**

```bash
python -m SimpleHTTPServer 8000
```

Access at: `http://localhost:8000`

---

### NFS - Network File System

**Client setup:**

**Mount NFS share:**

```bash
sudo mount server:/share /mnt/nfs
```

**Permanent mount in /etc/fstab:**

```
server:/share  /mnt/nfs  nfs  defaults  0  0
```

---

### Samba - Windows Sharing

**Install:**

```bash
sudo apt install samba
```

**Configure:**

```bash
sudo nano /etc/samba/smb.conf
```

**Set password:**

```bash
sudo smbpasswd -a username
```

**Restart service:**

```bash
sudo systemctl restart smbd
```

**Access from Linux:**

```bash
smbclient //server/share -U username
```

**Mount:**

```bash
sudo mount -t cifs //server/share /mnt/share -o username=user,password=pass
```

---

## Network Troubleshooting

### ping - Test Connectivity

**Basic ping:**

```bash
ping google.com
```

**Limited count:**

```bash
ping -c 4 google.com
```

**Output:**

```
64 bytes from 172.217.14.196: icmp_seq=1 ttl=117 time=11.4 ms
```

**Fields:**

- **icmp_seq** - Packet sequence
- **ttl** - Time to live (hops left)
- **time** - Round-trip time

---

### traceroute - Trace Route

**Trace path:**

```bash
traceroute google.com
```

**Shows:**

- Each hop (router)
- Response times
- Where packets fail

**Example output:**

```
 1  192.168.1.1 (192.168.1.1)  1.234 ms
 2  10.0.0.1 (10.0.0.1)  5.678 ms
 3  * * *
```

`* * *` means hop didn't respond.

---

### netstat - Network Statistics

**All connections:**

```bash
netstat -tuln
```

**Options:**

- `-t` - TCP
- `-u` - UDP
- `-l` - Listening
- `-n` - Numeric (no DNS)
- `-p` - Show programs

**Active connections:**

```bash
netstat -tun
```

**Listening ports:**

```bash
netstat -tln
```

**With programs:**

```bash
sudo netstat -tulpn
```

---

### ss - Socket Statistics

**Modern alternative to netstat.**

**All sockets:**

```bash
ss -tuln
```

**Listening TCP:**

```bash
ss -tln
```

**With process info:**

```bash
sudo ss -tulpn
```

**Specific port:**

```bash
ss -tulpn | grep :80
```

---

### nmap - Network Scanner

**Scan host:**

```bash
nmap 192.168.1.1
```

**Scan range:**

```bash
nmap 192.168.1.0/24
```

**Scan specific ports:**

```bash
nmap -p 22,80,443 192.168.1.1
```

**Service detection:**

```bash
nmap -sV 192.168.1.1
```

---

### tcpdump - Packet Analyzer

**Capture on interface:**

```bash
sudo tcpdump -i eth0
```

**Save to file:**

```bash
sudo tcpdump -i eth0 -w capture.pcap
```

**Read from file:**

```bash
tcpdump -r capture.pcap
```

**Filter by host:**

```bash
sudo tcpdump host 192.168.1.1
```

**Filter by port:**

```bash
sudo tcpdump port 80
```

**HTTP traffic:**

```bash
sudo tcpdump -i eth0 'tcp port 80'
```

---

### nc (netcat) - Network Tool

**Test port:**

```bash
nc -zv hostname 80
```

**Listen on port:**

```bash
nc -l 1234
```

**Connect to port:**

```bash
nc hostname 1234
```

**Transfer file:**

**Receiver:**

```bash
nc -l 1234 > received_file
```

**Sender:**

```bash
nc hostname 1234 < file_to_send
```

---

### curl - Transfer Data

**GET request:**

```bash
curl https://example.com
```

**Download file:**

```bash
curl -O https://example.com/file.zip
```

**POST data:**

```bash
curl -X POST -d "data=value" https://example.com/api
```

**Headers:**

```bash
curl -H "Content-Type: application/json" https://example.com
```

**Follow redirects:**

```bash
curl -L https://example.com
```

---

### wget - Download Files

**Download file:**

```bash
wget https://example.com/file.zip
```

**Resume download:**

```bash
wget -c https://example.com/largefile.iso
```

**Download to specific file:**

```bash
wget -O myfile.zip https://example.com/file.zip
```

**Recursive download:**

```bash
wget -r https://example.com/directory/
```

**Limit rate:**

```bash
wget --limit-rate=200k https://example.com/file.zip
```

---

## DNS

### DNS Hierarchy

1. **Root servers** (.)
2. **TLD servers** (.com, .org, .net)
3. **Authoritative servers** (google.com)
4. **Local resolver** (your computer/router)

---

### /etc/hosts

**Local name resolution:**

```bash
sudo nano /etc/hosts
```

**Format:**

```
127.0.0.1       localhost
192.168.1.100   myserver.local
```

---

### /etc/resolv.conf

**DNS server configuration:**

```bash
cat /etc/resolv.conf
```

**Example:**

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

---

### nslookup - Query DNS

**Basic lookup:**

```bash
nslookup google.com
```

**Specific server:**

```bash
nslookup google.com 8.8.8.8
```

**Reverse lookup:**

```bash
nslookup 8.8.8.8
```

---

### dig - DNS Tool

**Basic query:**

```bash
dig google.com
```

**Short answer:**

```bash
dig google.com +short
```

**Specific record:**

```bash
dig google.com A
dig google.com MX
dig google.com NS
dig google.com TXT
```

**Reverse lookup:**

```bash
dig -x 8.8.8.8
```

**Trace resolution:**

```bash
dig google.com +trace
```

---

### host - DNS Lookup

**Simple lookup:**

```bash
host google.com
```

**All records:**

```bash
host -a google.com
```

**Specific type:**

```bash
host -t MX google.com
```

---

## Troubleshooting Workflow

**Step-by-step network diagnosis:**

**1. Check interface:**

```bash
ip link show
```

**2. Check IP address:**

```bash
ip addr show
```

**3. Check default gateway:**

```bash
ip route show
```

**4. Ping gateway:**

```bash
ping -c 4 192.168.1.1
```

**5. Ping external IP:**

```bash
ping -c 4 8.8.8.8
```

**6. Test DNS:**

```bash
nslookup google.com
```

**7. Ping domain:**

```bash
ping -c 4 google.com
```

**8. Trace route:**

```bash
traceroute google.com
```

**9. Check listening ports:**

```bash
sudo ss -tulpn
```

**10. Capture packets:**

```bash
sudo tcpdump -i eth0 -c 10
```

---

## Summary

✅ **Network basics** - TCP/IP, addressing  
✅ **Configuration** - ifconfig, ip commands  
✅ **Routing** - Route tables, gateways  
✅ **File sharing** - scp, rsync, NFS, Samba  
✅ **Troubleshooting** - ping, traceroute, netstat  
✅ **DNS** - nslookup, dig, host  
✅ **Tools** - tcpdump, nc, curl, wget

---

## Quick Reference

```bash
# Interface
ip addr show                # Show IP addresses
sudo ip addr add IP dev eth0 # Add IP
sudo ip link set eth0 up    # Bring up interface

# Routing
ip route show               # Show routes
sudo ip route add via GW    # Add route

# Testing
ping host                   # Test connectivity
traceroute host             # Trace route
nslookup domain             # DNS lookup
dig domain                  # Detailed DNS
```
