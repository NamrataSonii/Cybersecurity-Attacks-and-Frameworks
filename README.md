# Cybersecurity Attacks and Frameworks

> A reference repo showcasing my understanding of cybersecurity attack vectors and security frameworks — built to demonstrate security fundamentals for learning purposes.

---

## What's Covered

### [Web Application Attacks](./WebApplicationAttackTypes.md)

Covers vulnerabilities and attack techniques targeting web applications directly.

| Attack | Description |
|--------|-------------|
| Open Redirect | Redirects users to malicious sites by abusing unvalidated URL parameters |
| HTTP Parameter Pollution (HPP) | Manipulates HTTP requests by injecting duplicate parameters |
| Cross-Site Request Forgery (CSRF) | Forces authenticated users to unknowingly perform actions |
| HTML Injection | Injects raw HTML into web pages to manipulate what users see |
| Cross-Site Scripting (XSS) | Injects malicious JavaScript executed by victim browsers |
| Template Injection (SSTI) | Exploits server-side template engines to execute code |
| SQL Injection (SQLi) | Injects SQL statements to access or manipulate databases |
| Server Side Request Forgery (SSRF) | Uses the server to make unauthorized HTTP requests |
| Remote Code Execution (RCE) | Injects and executes arbitrary code on the target system |
| Subdomain Takeover | Claims an abandoned subdomain of a legitimate site |
| Insecure Direct Object Reference (IDOR) | Accesses objects the attacker should not be authorized to reach |
| XML External Entity (XXE) | Exploits XML parsers to read files or make internal requests |

---

### [Network Attacks](./NetworkAttacks.md)

Covers attacks targeting network infrastructure, traffic, and protocols.

**Denial of Service (DoS)**

| Attack | Description |
|--------|-------------|
| Volume-Based (UDP/ICMP Flood) | Overwhelms bandwidth with massive traffic |
| Protocol Attacks (SYN Flood, Ping of Death) | Exploits protocol weaknesses to exhaust server resources |
| Application Layer (HTTP Flood, Slowloris) | Targets specific applications to crash or slow them |
| DDoS (Botnet, Amplification) | Uses many machines to attack a single target |
| Resource Exhaustion | Repeatedly requests resources until the system crashes |
| Reflective Attacks (DNS/NTP Reflection) | Floods victim via third-party servers using spoofed source IP |

**Man-in-the-Middle and Interception**

| Attack | Description |
|--------|-------------|
| Man-in-the-Middle (MitM) | Secretly intercepts communications between two parties |
| Spoofing | Fakes source identity or address to impersonate a trusted entity |
| ARP Spoofing | Poisons ARP cache to intercept local network traffic |
| DNS Spoofing | Injects false DNS records to redirect users |
| Session Hijacking | Steals session tokens to impersonate authenticated users |
| Packet Sniffing | Captures plaintext packets traveling across the network |
| IP Spoofing | Forges source IP address to conceal identity or amplify attacks |

**DNS Attacks**

| Attack | Description |
|--------|-------------|
| DNS Cache Poisoning | Corrupts resolver cache to redirect users |
| DNS Amplification | Uses DNS servers to flood victim with amplified traffic |
| DNS Tunneling | Encodes data inside DNS queries to bypass firewalls |
| DNS Hijacking | Modifies DNS settings to redirect an entire domain |

---

### [MITRE ATT&CK Framework](./MITREAttack.md)

Covers the 15 MITRE ATT&CK tactics that describe adversary behavior across the full attack lifecycle.

| Tactic | Phase | Goal |
|--------|-------|------|
| Reconnaissance | Pre-attack | Gather target information |
| Resource Development | Pre-attack | Acquire tools, infrastructure, and accounts |
| Initial Access | Entry | Gain the first foothold in the network |
| Execution | Post-access | Run adversary-controlled code |
| Persistence | Post-access | Maintain access across interruptions |
| Privilege Escalation | Post-access | Gain higher-level permissions |
| Stealth | Post-access | Blend in and avoid detection |
| Defense Impairment | Post-access | Degrade or disable security controls |
| Credential Access | Post-access | Steal account credentials |
| Discovery | Post-access | Map systems and the internal network |
| Lateral Movement | Post-access | Pivot through the network toward the target |
| Collection | Pre-exfiltration | Gather relevant data from the environment |
| Command and Control | Ongoing | Communicate with and control compromised systems |
| Exfiltration | Data theft | Remove stolen data from the network |
| Impact | End goal | Disrupt, destroy, or manipulate systems and data |

---

### [Cyber Kill Chain](./CyberKillChain.md)

Covers the 7-phase Cyber Kill Chain framework developed by Lockheed Martin as part of their Intelligence Driven Defense model.

| Phase | Name | Goal |
|-------|------|------|
| 1 | Reconnaissance | Research and identify the target |
| 2 | Weaponization | Build a deliverable exploit payload |
| 3 | Delivery | Transmit the weapon to the target |
| 4 | Exploitation | Trigger code execution on the victim system |
| 5 | Installation | Establish persistent access via backdoor or RAT |
| 6 | Command and Control | Gain remote control of the compromised host |
| 7 | Actions on Objectives | Execute the primary attack goal |

> Key principle: An attacker must complete every phase to succeed. Disrupting even one phase breaks the entire chain.

---

## Framework Comparison

| Aspect | Cyber Kill Chain | MITRE ATT&CK |
|--------|-----------------|--------------|
| Created by | Lockheed Martin | MITRE Corporation |
| Structure | 7 sequential phases | 14 tactics, hundreds of techniques |
| Focus | High-level attack lifecycle | Granular techniques and sub-techniques |
| Best used for | Understanding attack stages and disruption points | Mapping specific attacker behaviors to detections |
| Granularity | Phase level | Technique and sub-technique level |

---

## Attack Category Overview

```
Cybersecurity Attacks
|
|-- Web Application Attacks
|     |-- Injection           (SQLi, XSS, SSTI, RCE, XXE, HTML Injection)
|     |-- Request Forgery     (CSRF, SSRF)
|     |-- Input Validation    (Open Redirect, HPP)
|     |-- Access Control      (IDOR, Subdomain Takeover)
|
|-- Network Attacks
      |-- Denial of Service   (DoS, DDoS, Flood attacks, Exhaustion)
      |-- Interception        (MitM, Packet Sniffing, Session Hijacking)
      |-- Spoofing            (ARP, DNS, IP Spoofing)
      |-- DNS Attacks         (Poisoning, Tunneling, Hijacking, Amplification)

Security Frameworks
|
|-- MITRE ATT&CK             (15 tactics across the full adversary lifecycle)
|-- Cyber Kill Chain         (7 sequential phases — Lockheed Martin)
```

---

## How to Use This Repo

Each file is self-contained and can be read independently. Suggested reading order if you are new to cybersecurity:

1. Start with [Cyber Kill Chain](./CyberKillChain.md) — understand the big picture of how attacks unfold
2. Then read [MITRE ATT&CK Framework](./MITREAttack.md) — map attacker behavior in more detail
3. Then dive into [Web Application Attacks](./WebApplicationAttackTypes.md) — understand specific attack techniques
4. Then read [Network Attacks](./NetworkAttacks.md) — understand infrastructure-level attacks

---

## Contributing

Contributions are welcome. You can:
- Add new attacks or frameworks
- Fix errors or outdated information
- Improve explanations, examples, or diagrams
- Suggest better organization

Please open an **issue** or submit a **pull request**.

---
