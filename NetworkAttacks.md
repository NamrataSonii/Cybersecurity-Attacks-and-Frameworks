# Network Attacks

> A reference guide covering common network-level attacks — built to demonstrate understanding of offensive security and network threat concepts.

---

## Table of Contents

- [Denial of Service (DoS)](#denial-of-service-dos)
  - [Volume-Based Attacks](#1-volume-based-attacks)
  - [Protocol Attacks](#2-protocol-attacks)
  - [Application Layer Attacks](#3-application-layer-attacks)
  - [Distributed Denial-of-Service (DDoS)](#4-distributed-denial-of-service-ddos)
  - [Resource Exhaustion](#5-resource-exhaustion)
  - [Reflective Attacks](#6-reflective-attacks)
- [Man-in-the-Middle (MitM)](#man-in-the-middle-mitm)
  - [Spoofing](#spoofing)
  - [ARP Spoofing](#arp-spoofing)
  - [DNS Spoofing](#dns-spoofing)
  - [Session Hijacking](#session-hijacking)
- [Packet Sniffing](#packet-sniffing)
- [IP Spoofing](#ip-spoofing)
- [DNS Attacks](#dns-attacks)
- [Quick Reference Table](#quick-reference-table)

---

## Denial of Service (DoS)

### What is it?
A Denial of Service (DoS) attack is an attempt to **interrupt a website or network's operations** by overwhelming it with traffic. The attacker sends an enormous amount of requests to the target server, causing it to slow down or crash — making it inaccessible to legitimate users.

### Attack Flow

```
Attacker sends massive volume of requests
              |
              v
Target server becomes overwhelmed
              |
              v
Server slows down or crashes
              |
              v
Legitimate users cannot access the service
```

---

### 1. Volume-Based Attacks

**What it is:**
Flood a network with excessive data, overpowering its bandwidth and rendering the network unusable.

**Examples:**

| Attack | How it works |
|--------|-------------|
| **UDP Flood** | Attacker sends large numbers of UDP packets to random ports on the target server, forcing it to handle all requests until it slows down or stops serving legitimate traffic |
| **ICMP Flood** | Attacker overwhelms the target with ICMP Echo Request (ping) packets, consuming all available bandwidth |

---

### 2. Protocol Attacks

**What it is:**
Exploit weaknesses in network protocols to consume server resources rather than bandwidth.

**Examples:**

| Attack | How it works |
|--------|-------------|
| **SYN Flood** | Attacker sends many SYN requests but never completes the TCP three-way handshake, leaving the server with a large number of half-open connections until resources are exhausted |
| **Ping of Death** | Attacker sends oversized or malformed packets to crash or disrupt the target server |

**SYN Flood — TCP Handshake Abuse:**
```
Normal Handshake:       SYN Flood Attack:
Client -> SYN           Attacker -> SYN (x10000)
Server -> SYN-ACK       Server -> SYN-ACK (x10000, waiting...)
Client -> ACK           Attacker -> never responds
[Connection established] [Server holds 10000 half-open connections]
```

---

### 3. Application Layer Attacks

**What it is:**
Target specific applications or services to crash them or make them very slow, rather than targeting the network itself.

**Examples:**

| Attack | How it works |
|--------|-------------|
| **HTTP Flood** | Attacker sends massive numbers of HTTP requests to a web server, consuming its processing resources |
| **Slowloris** | Attacker opens many connections to the server and sends incomplete HTTP requests, keeping connections alive and preventing the server from handling new legitimate requests |

---

### 4. Distributed Denial-of-Service (DDoS)

**What it is:**
DDoS attacks use **multiple systems** — often a network of compromised computers called a **botnet** — to simultaneously attack a single target, making it much harder to block than a standard DoS.

**Examples:**

| Attack | How it works |
|--------|-------------|
| **Amplification Attack** | Attacker uses services like DNS to send a small query that generates a large response, flooding the victim with amplified data |
| **Botnet-Based Attack** | Many infected computers (bots) are coordinated to send attack traffic from multiple sources simultaneously |

**DDoS vs DoS:**

| | DoS | DDoS |
|-|-----|------|
| **Sources** | Single attacker machine | Multiple machines (botnet) |
| **Scale** | Limited | Massive |
| **Harder to block?** | No | Yes — traffic comes from many IPs |

---

### 5. Resource Exhaustion

**What it is:**
The attacker **repeatedly requests access to a resource**, eventually overloading the web application until it slows down and crashes — making the resource inaccessible to legitimate users.

**Attack Flow:**
```
Attacker repeatedly requests a resource (login page, API, file, etc.)
              |
              v
Application processes each request, consuming memory/CPU
              |
              v
Resources are fully consumed
              |
              v
Application slows down and crashes
              |
              v
Legitimate users cannot access the resource
```

---

### 6. Reflective Attacks

**What it is:**
The attacker sends requests to **third-party servers using the victim's IP address as the source**. The third-party servers unknowingly send their responses directly to the victim, overwhelming it with traffic the victim never requested.

**Examples:**

| Attack | How it works |
|--------|-------------|
| **DNS Reflection** | Attacker sends DNS queries with the victim's spoofed IP; the DNS server floods the victim with large responses |
| **NTP Reflection** | Works similarly but uses Network Time Protocol servers to amplify and redirect traffic at the victim |

**Reflective Attack Flow:**
```
Attacker spoofs victim's IP address
              |
              v
Sends requests to many third-party servers (DNS, NTP, etc.)
              |
              v
Third-party servers send responses to the victim's IP
              |
              v
Victim is flooded with unsolicited traffic
```



---

## Man-in-the-Middle (MitM)

### What is it?
A Man-in-the-Middle attack occurs when an attacker **secretly intercepts and potentially alters communications** between two parties who believe they are communicating directly with each other. Neither party is aware the attacker is sitting in the middle.

### Attack Flow
```
Normal Communication:
Client <-----------> Server

MitM Attack:
Client <---> Attacker <---> Server
             (intercepts, reads, or modifies traffic)
```

### Impact
- Eavesdropping on sensitive communications
- Credential and session token theft
- Data manipulation in transit
- Injecting malicious content into responses

---

### Spoofing

**What is it?**
Spoofing is the act of **disguising communication or identity** by faking the source address, identity, or data to appear as a trusted entity. It is the foundation of many MitM attacks.

**Types:**

| Type | What is faked |
|------|--------------|
| IP Spoofing | Source IP address |
| ARP Spoofing | MAC address mapping |
| DNS Spoofing | DNS resolution responses |
| Email Spoofing | Sender email address |

---

### ARP Spoofing

**What is it?**
ARP Spoofing (also called ARP Poisoning) occurs when an attacker **sends fake ARP (Address Resolution Protocol) messages** onto a local network, linking the attacker's MAC address to the IP address of a legitimate host.

**How it works:**
```
Normal ARP:
Host A asks: "Who has IP 192.168.1.1?"
Router replies: "I do. My MAC is AA:BB:CC:DD:EE:FF"

ARP Spoofing:
Attacker broadcasts fake ARP reply:
"IP 192.168.1.1 is at MY MAC address (attacker's MAC)"
Host A updates its ARP cache with the attacker's MAC
All traffic meant for the router now goes to the attacker
```

**Impact:**
- Intercepts all traffic on the local network segment
- Enables session hijacking and credential theft
- Can be used to launch further MitM attacks

---

### DNS Spoofing

**What is it?**
DNS Spoofing (also called DNS Cache Poisoning) involves **injecting malicious DNS records** into a resolver's cache, causing users to be redirected to fraudulent websites without their knowledge.

**How it works:**
```
Normal DNS Resolution:
User types: www.bank.com
DNS Server responds: IP = 93.184.216.34 (real server)
User connects to real bank website

DNS Spoofing:
Attacker poisons DNS cache with fake record:
DNS Server responds: IP = 10.0.0.99 (attacker's server)
User connects to fake bank website and enters credentials
```

**Impact:**
- Redirects users to phishing/malicious sites
- Credential and sensitive data theft
- Malware distribution via fake download pages

---

### Session Hijacking

**What is it?**
Session hijacking occurs when an attacker **steals a valid session token** to impersonate a legitimate user who is already authenticated, without needing their credentials.

**How it works:**
```
Step 1: Victim logs in -> Server issues session token (e.g. cookie)
Step 2: Attacker intercepts or steals the session token
        (via packet sniffing, XSS, MitM, etc.)
Step 3: Attacker uses the stolen token to make requests
        as if they are the authenticated victim
Step 4: Server accepts requests — it cannot distinguish
        the attacker from the real user
```

**Common Methods of Token Theft:**
- Packet sniffing on unencrypted connections
- Cross-Site Scripting (XSS) to steal cookies
- ARP Spoofing to intercept traffic

**Prevention:**
- Use HTTPS for all communications
- Set `HttpOnly` and `Secure` flags on cookies
- Implement session expiry and re-authentication

---

## Packet Sniffing

### What is it?
Packet sniffing is the process of **capturing and inspecting data packets** as they travel across a network. While it has legitimate uses (network diagnostics), attackers use it to intercept sensitive information transmitted in plaintext.

### How it works
```
Data travels across the network as packets
              |
              v
Attacker places a sniffer (tool/software) on the network
              |
              v
Sniffer captures packets passing through
              |
              v
Attacker reads contents — credentials, emails, session tokens, etc.
```

### Active vs Passive Sniffing

| Type | Description |
|------|-------------|
| **Passive Sniffing** | Listens silently on the network without sending any traffic — harder to detect |
| **Active Sniffing** | Involves injecting traffic (e.g. ARP spoofing) to redirect packets through the attacker |

### What attackers can capture
- Usernames and passwords sent over HTTP, FTP, Telnet
- Session tokens and cookies
- Email content
- Unencrypted files and messages

### Prevention
- Use encrypted protocols (HTTPS, SFTP, SSH) instead of HTTP, FTP, Telnet
- Use VPNs on untrusted networks
- Implement network segmentation

---

## IP Spoofing

### What is it?
IP Spoofing involves **forging the source IP address** in a packet header to disguise the attacker's identity or impersonate another system.

### How it works
```
Normal Packet:
[Source IP: Attacker's real IP] -> [Destination: Target]

Spoofed Packet:
[Source IP: Fake/Victim's IP]  -> [Destination: Target]
Target believes the packet came from a trusted/different source
```

### Common Uses by Attackers

| Use Case | Description |
|----------|-------------|
| **DoS/DDoS** | Hide the attacker's real IP during flood attacks |
| **Reflective Attacks** | Use victim's IP as source so responses flood the victim |
| **Bypassing Firewalls** | Forge a trusted IP to pass IP-based access controls |
| **Identity Concealment** | Avoid detection and attribution |

### Prevention
- Ingress and egress filtering on routers
- Use cryptographic authentication protocols
- Monitor for packets with impossible or suspicious source IPs

---

## DNS Attacks

### What is it?
DNS attacks target the **Domain Name System** — the internet's phonebook that translates domain names to IP addresses. Compromising DNS can silently redirect users and disrupt services.

### Types of DNS Attacks

#### DNS Cache Poisoning
Corrupts a DNS resolver's cache with false records, redirecting users to malicious sites. (See DNS Spoofing above for full details.)

#### DNS Amplification (DDoS)
```
Attacker sends small DNS query with victim's spoofed IP
              |
              v
DNS server sends a much larger response to the victim
              |
              v
Victim is flooded with amplified traffic from many DNS servers
```

#### DNS Tunneling
Encodes non-DNS data (commands, files) inside DNS queries and responses to **bypass firewalls and exfiltrate data** covertly.

```
Attacker encodes data inside DNS query:
data.exfiltrated.attacker-domain.com -> carries hidden payload
DNS server resolves it, passing data through firewall undetected
```

#### DNS Hijacking
The attacker **modifies DNS settings** at the router or registrar level, redirecting all DNS queries for a domain to an attacker-controlled server.

### DNS Attack Comparison

| Attack | What is Compromised | Impact |
|--------|-------------------|--------|
| **Cache Poisoning** | DNS resolver cache | User redirection, phishing |
| **Amplification** | DNS servers used as reflectors | DDoS via bandwidth flood |
| **Tunneling** | DNS protocol | Data exfiltration, C2 communication |
| **Hijacking** | DNS settings (router/registrar) | Full domain redirection |

---

## Quick Reference Table

| Attack | Category | Method | Impact |
|--------|----------|--------|--------|
| **UDP Flood** | DoS - Volume | Floods ports with UDP packets | Bandwidth exhaustion |
| **ICMP Flood** | DoS - Volume | Overwhelms with ping requests | Bandwidth exhaustion |
| **SYN Flood** | DoS - Protocol | Half-open TCP connections | Server resource exhaustion |
| **Ping of Death** | DoS - Protocol | Oversized/malformed packets | Server crash |
| **HTTP Flood** | DoS - Application | Massive HTTP requests | Web server overload |
| **Slowloris** | DoS - Application | Incomplete HTTP requests hold connections | Web server connection exhaustion |
| **DDoS (Botnet)** | DoS - Distributed | Many machines attack one target | Massive service disruption |
| **Amplification** | DoS - Distributed | Small query generates large response | Bandwidth flood |
| **Resource Exhaustion** | DoS - Application | Repeated resource requests | Server/app crash |
| **DNS Reflection** | DoS - Reflective | Spoofed IP floods via DNS servers | Bandwidth exhaustion |
| **NTP Reflection** | DoS - Reflective | Spoofed IP floods via NTP servers | Bandwidth exhaustion |
| **MitM** | Interception | Sits between two communicating parties | Eavesdropping, data theft |
| **Spoofing** | Deception | Fakes source identity or address | Trust abuse, further attacks |
| **ARP Spoofing** | MitM | Fake ARP replies poison local cache | Traffic interception |
| **DNS Spoofing** | MitM | Fake DNS records redirect users | Phishing, credential theft |
| **Session Hijacking** | MitM | Stolen session token used to impersonate user | Unauthorized access |
| **Packet Sniffing** | Interception | Captures plaintext packets on network | Credential and data theft |
| **IP Spoofing** | Deception | Forges source IP address | Identity concealment, DoS amplification |
| **DNS Cache Poisoning** | DNS | Corrupts resolver cache | User redirection |
| **DNS Amplification** | DNS / DoS | Uses DNS servers to flood victim | DDoS via bandwidth flood |
| **DNS Tunneling** | DNS | Encodes data inside DNS queries | Data exfiltration |
| **DNS Hijacking** | DNS | Modifies DNS settings at source | Full domain redirection |

---

## Contributing

Found an error or want to add more attacks? Contributions are welcome!
Please open an **issue** or submit a **pull request**.

---

