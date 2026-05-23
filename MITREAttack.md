# MITRE ATT&CK Framework — Tactics

> A reference guide covering the MITRE ATT&CK framework tactics — the categories of adversary behavior used to describe how attackers operate across the full attack lifecycle.

---

## Table of Contents

- [What is MITRE ATT&CK?](#what-is-mitre-attck)
- [Reconnaissance](#reconnaissance)
- [Resource Development](#resource-development)
- [Initial Access](#initial-access)
- [Execution](#execution)
- [Persistence](#persistence)
- [Privilege Escalation](#privilege-escalation)
- [Stealth](#stealth)
- [Defense Impairment](#defense-impairment)
- [Credential Access](#credential-access)
- [Discovery](#discovery)
- [Lateral Movement](#lateral-movement)
- [Collection](#collection)
- [Command and Control](#command-and-control)
- [Exfiltration](#exfiltration)
- [Impact](#impact)
- [Attack Lifecycle Overview](#attack-lifecycle-overview)
- [Quick Reference Table](#quick-reference-table)

---

## What is MITRE ATT&CK?

MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) is a globally recognized **knowledge base of adversary behavior** based on real-world observations. It categorizes the tactics (the "why") and techniques (the "how") that attackers use across the full lifecycle of a cyberattack.

It is widely used by:
- Red teams to simulate realistic attack scenarios
- Blue teams to identify detection gaps
- Security engineers to build better defenses
- Threat intelligence analysts to track threat actor behavior

---

## Reconnaissance

Reconnaissance involves adversaries **actively or passively gathering information** to support targeting. This is the earliest phase of an attack — before any system is touched.

### What attackers collect
- Details about the victim organization and its structure
- Infrastructure information (IP ranges, domains, services)
- Staff and personnel information (emails, roles, LinkedIn profiles)

### Active vs Passive Reconnaissance

| Type | Description | Detection Risk |
|------|-------------|---------------|
| **Passive** | Gathering info without directly interacting with the target (e.g. OSINT, public records) | Low |
| **Active** | Directly interacting with target systems (e.g. port scanning, probing) | Higher |

### How it supports later phases
- Identifies entry points for **Initial Access**
- Helps prioritize **post-compromise objectives**
- Drives further, more targeted Reconnaissance

---

## Resource Development

Resource Development involves adversaries **creating, purchasing, or stealing resources** to support their attack operations before targeting begins.

### Types of resources acquired

| Resource | Examples |
|----------|---------|
| **Infrastructure** | Domains, servers, VPNs, botnets |
| **Accounts** | Email accounts for phishing, social media accounts |
| **Capabilities** | Malware, exploit kits, code signing certificates |

### How it supports later phases
- Purchased domains support **Command and Control**
- Email accounts enable **phishing** during Initial Access
- Stolen code signing certificates help with **Defense Evasion**

---

## Initial Access

Initial Access consists of techniques adversaries use to **gain their first foothold** within a target network.

### Common techniques

| Technique | Description |
|-----------|-------------|
| **Spearphishing** | Targeted phishing emails crafted for specific individuals |
| **Exploiting public-facing applications** | Attacking vulnerabilities in web servers, VPNs, or other exposed services |
| **Valid accounts** | Using stolen or purchased credentials to log in legitimately |
| **External remote services** | Abusing RDP, VPNs, or other remote access services |

### Attack Flow
```
Attacker identifies entry point (via Reconnaissance)
              |
              v
Delivers exploit or phishing payload
              |
              v
Foothold established inside the network
              |
              v
Moves to Execution, Persistence, or Discovery
```

---

## Execution

Execution consists of techniques that result in **adversary-controlled code running** on a local or remote system.

### Key points
- Often paired with techniques from other tactics to achieve broader goals
- Used to explore a network, steal data, or deploy further payloads

### Example
```
Attacker gains Initial Access
              |
              v
Uses remote access tool to run PowerShell script
              |
              v
Script performs Remote System Discovery
              |
              v
Attacker maps the internal network
```

### Common execution methods
- PowerShell and command-line scripts
- Scheduled tasks and cron jobs
- Malicious macros in Office documents
- Exploiting interpreter environments (Python, Bash, etc.)

---

## Persistence

Persistence consists of techniques adversaries use to **maintain access to systems** across restarts, credential changes, and other interruptions.

### Why it matters
Without persistence, an attacker loses access the moment a system reboots or a password is changed. Persistence ensures the attacker can always return.

### Common techniques
- Replacing or hijacking legitimate code
- Adding malicious startup code or services
- Creating new user accounts with backdoor access
- Modifying scheduled tasks or registry run keys

> Note: Persistence techniques often overlap with Privilege Escalation, as OS features that allow persistence can also execute code in an elevated context.

---

## Privilege Escalation

Privilege Escalation involves techniques adversaries use to **gain higher-level permissions** on a system or network beyond what their initial access provides.

### Why attackers need it
Attackers often gain initial access with limited, unprivileged accounts. Elevated permissions are required to:
- Access sensitive data and systems
- Install tools and malware
- Disable security controls
- Move laterally across the network

### Levels of elevated access

| Level | Description |
|-------|-------------|
| **SYSTEM / root** | Highest OS-level privileges |
| **Local administrator** | Full control over the local machine |
| **Admin-like user account** | User with elevated but not full admin rights |
| **Specific function accounts** | Access to a particular system or function only |

### Common techniques
- Exploiting OS vulnerabilities or misconfigurations
- Abusing weak file or service permissions
- Token impersonation
- Kernel exploits

---

## Stealth

Stealth consists of techniques that **reduce the likelihood of detection** by blending in with legitimate activity or minimizing observable signals — without directly modifying security controls.

### Key characteristics
- Concealment through mimicry of normal operations
- Avoiding generating unusual logs or alerts
- Obfuscating activity to appear benign
- Defensive systems are left intact (unlike Defense Impairment)

### Goal
Remain **indistinguishable from legitimate activity** while continuing the attack undetected.

### Examples
- Using built-in OS tools (Living off the Land — LotL)
- Timing attacks to occur during business hours
- Encrypting or encoding malicious traffic
- Renaming malicious files to match system files

---

## Defense Impairment

Defense Impairment involves techniques that **directly degrade, disable, or undermine** security controls and monitoring mechanisms.

### How it differs from Stealth

| | Stealth | Defense Impairment |
|-|---------|--------------------|
| **Approach** | Blend in, avoid detection | Directly attack defensive systems |
| **Security controls** | Left intact | Disabled or degraded |
| **Goal** | Be invisible | Remove the ability to detect |

### Common techniques
- Disabling antivirus or endpoint detection tools
- Clearing event logs to remove evidence
- Stopping security monitoring services
- Modifying or deleting firewall rules

---

## Credential Access

Credential Access consists of techniques for **stealing account names and passwords** to use legitimate credentials for further access.

### Why it is valuable to attackers
Using legitimate credentials makes attackers:
- Harder to detect (activity looks normal)
- Able to access systems without exploiting vulnerabilities
- Capable of creating additional accounts for persistent access

### Common techniques

| Technique | Description |
|-----------|-------------|
| **Keylogging** | Records keystrokes to capture credentials as they are typed |
| **Credential dumping** | Extracts stored credentials from OS memory or files (e.g. LSASS, SAM database) |
| **Phishing** | Tricks users into entering credentials on fake login pages |
| **Brute force** | Systematically tries passwords until one works |

---

## Discovery

Discovery consists of techniques an adversary uses to **gain knowledge about the system and internal network** after gaining access.

### Purpose
- Observe and understand the environment
- Identify what systems and data exist
- Determine how to move toward the primary objective

### Common discovery activities
- Network scanning to find live hosts and open ports
- Enumerating user accounts and group memberships
- Identifying running processes and installed software
- Mapping file shares and accessible directories
- Checking for security tools and monitoring software

> Note: Native OS tools (e.g. `ipconfig`, `net user`, `whoami`, `ps`) are frequently used during Discovery to blend in with normal activity.

---

## Lateral Movement

Lateral Movement consists of techniques adversaries use to **move through a network** from their initial entry point toward their ultimate target.

### Why it is needed
The initially compromised system is rarely the target. Attackers must navigate through the network, pivoting across multiple systems and accounts to reach high-value assets.

### Attack Flow
```
Initial foothold on low-value system
              |
              v
Discovery — map the internal network
              |
              v
Identify path to target system
              |
              v
Pivot using credentials, tools, or exploits
              |
              v
Gain access to target system
```

### Common techniques
- Pass-the-Hash / Pass-the-Ticket attacks
- Using stolen credentials with RDP or SMB
- Deploying remote access tools on new systems
- Exploiting trust relationships between systems

---

## Collection

Collection consists of techniques adversaries use to **gather information and data** relevant to their objectives from within the compromised environment.

### Common data sources targeted

| Source | Examples |
|--------|---------|
| **Storage drives** | Local files, network shares, removable media |
| **Browsers** | Saved passwords, browsing history, cookies |
- **Email** — inbox contents, contacts, attachments |
| **Input capture** | Keystrokes, clipboard contents |
| **Screen capture** | Screenshots of active sessions |
| **Audio/Video** | Microphone and camera capture on compromised endpoints |

### Next step after collection
Data collected is typically either:
- **Exfiltrated** — stolen and sent outside the network
- **Used for further discovery** — to gain more knowledge about the target environment

---

## Command and Control

Command and Control (C2) consists of techniques adversaries use to **communicate with compromised systems** within the victim network to issue commands and receive data.

### Key goals
- Maintain remote control over compromised systems
- Issue commands to execute further attack stages
- Receive stolen data from the compromised environment

### Stealth in C2
Adversaries attempt to **mimic normal, expected traffic** to avoid detection by blending C2 communications with legitimate network activity.

### Common C2 channels

| Channel | Description |
|---------|-------------|
| **HTTP/HTTPS** | Blends with normal web traffic |
| **DNS** | Encodes commands in DNS queries (DNS Tunneling) |
| **Encrypted channels** | Custom encrypted protocols to evade inspection |
| **Cloud services** | Uses legitimate platforms (Google Drive, Dropbox) as C2 relays |

---

## Exfiltration

Exfiltration consists of techniques adversaries use to **steal data out of the target network** after it has been collected.

### How attackers avoid detection during exfiltration
- **Compression** — reduces file size and disguises content
- **Encryption** — hides the data from network inspection tools
- **Size limits** — sends data in small chunks to avoid triggering alerts

### Exfiltration channels

| Channel | Description |
|---------|-------------|
| **C2 channel** | Data sent out through the existing Command and Control connection |
| **Alternate channel** | Separate channel used specifically for data transfer (e.g. FTP, cloud storage) |

### Exfiltration Flow
```
Data collected from target environment
              |
              v
Packaged — compressed and/or encrypted
              |
              v
Transferred out via C2 or alternate channel
              |
              v
Received by attacker outside the network
```

---

## Impact

Impact consists of techniques adversaries use to **disrupt availability or compromise integrity** of systems and business processes.

### Goals of Impact techniques
- Destroy or tamper with data
- Disrupt business operations
- Provide cover for a confidentiality breach (distraction)
- Follow through on the attacker's end objective

### Important note
In some cases, **business processes may appear normal** but have been secretly altered to benefit the attacker — making Impact techniques particularly difficult to detect.

### Common Impact techniques

| Technique | Description |
|-----------|-------------|
| **Data destruction** | Deleting or overwriting files and databases |
| **Ransomware** | Encrypting data and demanding payment for decryption |
| **Defacement** | Altering websites or systems visibly |
| **Service disruption** | Taking down systems or services (similar to DoS) |
| **Data manipulation** | Subtly altering data to corrupt integrity without obvious destruction |

---

## Attack Lifecycle Overview

```
[1] Reconnaissance       — Gather information about the target
         |
         v
[2] Resource Development — Acquire tools, infrastructure, accounts
         |
         v
[3] Initial Access       — Gain first foothold in the network
         |
         v
[4] Execution            — Run malicious code on target systems
         |
         v
[5] Persistence          — Maintain access across interruptions
         |
         v
[6] Privilege Escalation — Gain higher-level permissions
         |
         v
[7] Stealth              — Blend in, avoid raising alerts
[8] Defense Impairment   — Disable security controls
         |
         v
[9] Credential Access    — Steal usernames and passwords
         |
         v
[10] Discovery           — Map the internal environment
         |
         v
[11] Lateral Movement    — Move toward the target system
         |
         v
[12] Collection          — Gather relevant data
         |
         v
[13] Command and Control — Maintain remote control
         |
         v
[14] Exfiltration        — Steal data out of the network
         |
         v
[15] Impact              — Disrupt, destroy, or manipulate
```

---

## Quick Reference Table

| Tactic | Phase | Goal |
|--------|-------|------|
| **Reconnaissance** | Pre-attack | Gather target information |
| **Resource Development** | Pre-attack | Acquire tools, infrastructure, accounts |
| **Initial Access** | Entry | Gain first foothold in the network |
| **Execution** | Post-access | Run adversary-controlled code |
| **Persistence** | Post-access | Maintain access across interruptions |
| **Privilege Escalation** | Post-access | Gain higher-level permissions |
| **Stealth** | Post-access | Blend in and avoid detection |
| **Defense Impairment** | Post-access | Degrade or disable security controls |
| **Credential Access** | Post-access | Steal account credentials |
| **Discovery** | Post-access | Map systems and internal network |
| **Lateral Movement** | Post-access | Pivot through the network toward target |
| **Collection** | Pre-exfiltration | Gather relevant data from the environment |
| **Command and Control** | Ongoing | Communicate with compromised systems |
| **Exfiltration** | Data theft | Remove stolen data from the network |
| **Impact** | End goal | Disrupt, destroy, or manipulate systems and data |

---

## Contributing

Found an error or want to add techniques under each tactic? Contributions are welcome!
Please open an **issue** or submit a **pull request**.

---
