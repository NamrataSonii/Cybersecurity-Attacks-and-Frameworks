# Cyber Kill Chain

---

## Table of Contents

- [What is the Cyber Kill Chain?](#what-is-the-cyber-kill-chain)
- [What is APT?](#what-is-apt)
- [The Seven Phases](#the-seven-phases)
  - [Phase 1 — Reconnaissance](#phase-1--reconnaissance)
  - [Phase 2 — Weaponization](#phase-2--weaponization)
  - [Phase 3 — Delivery](#phase-3--delivery)
  - [Phase 4 — Exploitation](#phase-4--exploitation)
  - [Phase 5 — Installation](#phase-5--installation)
  - [Phase 6 — Command and Control (C2)](#phase-6--command-and-control-c2)
  - [Phase 7 — Actions on Objectives](#phase-7--actions-on-objectives)
- [Kill Chain Attack Flow](#kill-chain-attack-flow)
- [Why the Kill Chain Model Matters for Defense](#why-the-kill-chain-model-matters-for-defense)
- [Courses of Action Matrix](#courses-of-action-matrix)
- [Cyber Kill Chain vs MITRE ATT&CK](#cyber-kill-chain-vs-mitre-attck)
- [Quick Reference Table](#quick-reference-table)

---

## What is the Cyber Kill Chain?

The Cyber Kill Chain was developed by Lockheed Martin as part of their **Intelligence Driven Defense** model. It is a framework for identifying and preventing cyber intrusion activity by modeling the phases an adversary must complete to achieve their objective.

The model is built on a core principle: an attacker must successfully progress through **every phase** of the chain to accomplish their goal. Disrupting even a single phase breaks the entire chain and stops the attack.

> "Kill chain analysis illustrates that the adversary must progress successfully through each stage of the chain before it can achieve its desired objective; just one mitigation disrupts the chain and the adversary."
> — Lockheed Martin, Intelligence-Driven Computer Network Defense

### Key benefits of the framework
- Gives defenders **visibility into each phase** of an attack
- Helps analysts understand **adversary tactics, techniques, and procedures (TTPs)**
- Enables defenders to build **proactive** rather than purely reactive defenses
- Supports **intelligence-driven** prioritization of security investments

---

## What is APT?

The Cyber Kill Chain was specifically designed to address **Advanced Persistent Threats (APT)**.

| Attribute | Description |
|-----------|-------------|
| **Advanced (A)** | Uses sophisticated tools, custom malware, and zero-day exploits that bypass conventional defenses |
| **Persistent (P)** | Conducts long-term, multi-year intrusion campaigns — patient and methodical |
| **Threat (T)** | Carried out by well-resourced adversaries with clear intent, opportunity, and capability |

APT actors target highly sensitive economic, proprietary, or national security information and continually adapt their operations based on the success or failure of previous attempts.

---

## The Seven Phases

### Phase 1 — Reconnaissance

**What happens:**
The adversary researches, identifies, and selects targets. This is the information-gathering phase before any attack begins.

**Common activities:**
- Crawling websites, conference proceedings, and mailing lists for email addresses
- Mapping social relationships and organizational structure
- Identifying technologies in use (web servers, software versions, frameworks)
- OSINT (Open Source Intelligence) gathering via LinkedIn, job postings, public records

**What attackers are looking for:**
- Entry points into the organization
- Staff names, roles, and email addresses for social engineering
- Technical vulnerabilities in public-facing infrastructure

**Defender actions:**
- Monitor for web crawling and scanning activity
- Limit publicly available technical information
- Track what information about your organization is exposed online

---

### Phase 2 — Weaponization

**What happens:**
The adversary creates an attack payload by **coupling an exploit with a remote access trojan (RAT)** or malware into a deliverable weapon — typically using an automated tool called a weaponizer.

**Common weaponized formats:**
- Malicious PDF documents
- Microsoft Office files with embedded macros
- Executable files disguised as legitimate software
- Archive files containing malware droppers

**Key point:**
No interaction with the victim occurs during this phase. The attacker is purely preparing their weapon based on what they learned during Reconnaissance.

**Defender actions:**
- Analyze malware artifacts to identify weaponization patterns
- Track attacker toolkits and weaponizer signatures
- Build detection for known malicious document types and behaviors

---

### Phase 3 — Delivery

**What happens:**
The weaponized payload is **transmitted to the target environment**. This is the first point of direct interaction between the attacker and the victim.

**Three most prevalent delivery vectors (as observed by LM-CIRT):**

| Vector | Description |
|--------|-------------|
| **Email attachments** | Spearphishing emails with malicious attachments tailored to the target |
| **Websites** | Drive-by downloads or malicious links in emails that direct victims to attacker-controlled pages |
| **USB removable media** | Physical delivery of infected drives, often used in air-gapped environments |

**Defender actions:**
- Email filtering and attachment sandboxing
- Web proxy filtering and URL reputation checks
- Controls on removable media usage
- User awareness training for phishing recognition

---

### Phase 4 — Exploitation

**What happens:**
After delivery, the weapon **triggers the attacker's code** on the victim's system. This is where the vulnerability is actually exploited.

**Common exploitation targets:**
- Application vulnerabilities (browser, PDF reader, Office suite)
- Operating system vulnerabilities
- Zero-day exploits with no available patch
- The user themselves — tricking them into enabling macros or running code
- OS features that auto-execute code (e.g. autorun)

**Key point:**
Exploitation does not always require a technical vulnerability. Social engineering — tricking a user into taking an action — is equally effective and often easier.

**Defender actions:**
- Patch management and vulnerability scanning
- Endpoint protection and application whitelisting
- Disable auto-execution features
- User training to recognize social engineering

---

### Phase 5 — Installation

**What happens:**
The attacker installs a **remote access trojan (RAT), backdoor, or persistent implant** on the victim system to maintain long-term access even after reboots or credential changes.

**Common persistence mechanisms:**
- Adding entries to registry run keys
- Installing malicious services or scheduled tasks
- Replacing or hijacking legitimate system files
- Installing web shells on compromised web servers

**Key point:**
Without installation, an attacker loses access the moment the compromised session ends. Persistence is what separates a one-time breach from a long-term intrusion.

**Defender actions:**
- Monitor for new services, scheduled tasks, and registry modifications
- File integrity monitoring on critical system files
- Host-based intrusion detection systems (HIDS)
- Endpoint detection and response (EDR) tools

---

### Phase 6 — Command and Control (C2)

**What happens:**
The compromised host **beacons out to an attacker-controlled server** to establish a communication channel. This gives the attacker remote, interactive control over the victim system.

**How C2 channels work:**
- Compromised hosts send periodic check-in signals to the C2 server
- The C2 server sends back commands for the implant to execute
- Attackers attempt to **mimic normal, legitimate traffic** to avoid detection

**Common C2 channels:**

| Channel | Why attackers use it |
|---------|---------------------|
| **HTTP/HTTPS** | Blends with normal web browsing traffic |
| **DNS tunneling** | DNS traffic is rarely inspected; encodes commands in queries |
| **Encrypted channels** | Prevents content inspection by security tools |
| **Legitimate cloud services** | Uses trusted platforms (e.g. Google Drive, Dropbox) as relay points |

**Defender actions:**
- Network traffic analysis and anomaly detection
- DNS monitoring for unusual query patterns
- Block known C2 infrastructure via threat intelligence feeds
- Inspect encrypted traffic where possible (SSL inspection)

---

### Phase 7 — Actions on Objectives

**What happens:**
With persistent access and C2 established, the adversary **takes actions to achieve their original goal**. This phase represents the attacker reaching their primary objective.

**Common objectives:**

| Objective | Description |
|-----------|-------------|
| **Data exfiltration** | Stealing sensitive files, credentials, intellectual property |
| **Lateral movement** | Pivoting to other systems within the network |
| **Privilege escalation** | Gaining higher-level access to reach more valuable targets |
| **Data destruction** | Deleting or corrupting data to cause damage |
| **Ransomware deployment** | Encrypting data and demanding payment |
| **Espionage** | Long-term, covert collection of sensitive information |
| **Disruption** | Disrupting business operations or critical infrastructure |

**Key point:**
Actions on Objectives may unfold over a long period — APT actors are known for their patience, often residing inside networks for months or years before executing their final objective.

**Defender actions:**
- Data loss prevention (DLP) tools
- Behavioral analytics to detect unusual data access patterns
- Network segmentation to limit lateral movement
- Incident response planning for containment and recovery

---

## Kill Chain Attack Flow

```
[Phase 1] Reconnaissance
          Attacker researches target — OSINT, scanning, social profiling
                        |
                        v
[Phase 2] Weaponization
          Creates malicious payload (malware + exploit)
                        |
                        v
[Phase 3] Delivery
          Sends payload via email, website, or USB
                        |
                        v
[Phase 4] Exploitation
          Payload triggers — vulnerability or user action exploited
                        |
                        v
[Phase 5] Installation
          Backdoor/RAT installed — persistent access established
                        |
                        v
[Phase 6] Command and Control (C2)
          Compromised host beacons out — attacker gains remote control
                        |
                        v
[Phase 7] Actions on Objectives
          Attacker executes goal — data theft, destruction, espionage
```

---

## Why the Kill Chain Model Matters for Defense

Traditional security approaches assume a breach has already occurred and focus on response after the fact. The Cyber Kill Chain shifts this thinking:

**Old approach:** Detect and respond after compromise
**Kill chain approach:** Detect and disrupt at any phase before the objective is reached

### The key insight
Because the attacker must complete **every phase** to succeed, defenders only need to disrupt **one phase** to stop the attack. This gives defenders multiple opportunities to intervene.

```
Phase 1  Phase 2  Phase 3  Phase 4  Phase 5  Phase 6  Phase 7
Recon -> Weapon -> Deliver -> Exploit -> Install -> C2 -> Objective
  |         |         |          |          |        |        |
 Stop      Stop      Stop       Stop       Stop     Stop     Stop
  here     here      here       here       here     here     here
     <- Any single disruption breaks the entire kill chain ->
```

### Intelligence feedback loop
Each detected intrusion attempt provides **indicators** (IP addresses, file hashes, behavioral patterns) that defenders can use to detect future attempts earlier in the chain — creating a continuous intelligence feedback loop that raises the cost and difficulty for the attacker over time.

---

## Courses of Action Matrix

For each phase, defenders have several categories of response available:

| Phase | Detect | Deny | Disrupt | Degrade | Deceive | Destroy |
|-------|--------|------|---------|---------|---------|---------|
| Reconnaissance | Web analytics, firewall logs | Firewall ACLs | — | — | Honeypots | — |
| Weaponization | Malware analysis | NIDS signatures | — | — | — | — |
| Delivery | Antivirus, audit logs | Proxy filters | Inline AV | — | — | — |
| Exploitation | HIDS, patches | DEP, patching | — | — | — | — |
| Installation | HIDS, checksums | Chroot jail | AV | — | — | — |
| Command and Control | Network IDS | Firewall/DNS | DNS redirect | — | DNS sinkhole | — |
| Actions on Objectives | Audit logs, DLP | Honeypots | — | QoS throttle | — | — |

---

## Cyber Kill Chain vs MITRE ATT&CK

Both frameworks model adversary behavior but serve different purposes:

| Aspect | Cyber Kill Chain | MITRE ATT&CK |
|--------|-----------------|--------------|
| **Created by** | Lockheed Martin | MITRE Corporation |
| **Structure** | 7 sequential phases | 14 tactics, hundreds of techniques |
| **Focus** | High-level attack lifecycle | Granular techniques and sub-techniques |
| **Best used for** | Understanding attack stages and disruption points | Mapping specific attacker behaviors to detections |
| **Granularity** | Low (phase level) | High (technique and sub-technique level) |
| **Complementary?** | Yes — Kill Chain phases map to ATT&CK tactics |

> The two frameworks are complementary. The Cyber Kill Chain provides the high-level lifecycle structure; MITRE ATT&CK fills in the specific techniques at each stage.

---

## Quick Reference Table

| Phase | Number | Goal | Key Defender Action |
|-------|--------|------|-------------------|
| Reconnaissance | 1 | Gather information about the target | Monitor scanning activity, limit public exposure |
| Weaponization | 2 | Build a deliverable exploit payload | Analyze malware artifacts, track weaponizer patterns |
| Delivery | 3 | Transmit payload to target | Email filtering, web proxies, USB controls |
| Exploitation | 4 | Trigger code execution on victim | Patch management, application whitelisting |
| Installation | 5 | Establish persistent access | HIDS, file integrity monitoring, EDR |
| Command and Control | 6 | Gain remote control of compromised host | Network anomaly detection, DNS monitoring |
| Actions on Objectives | 7 | Execute the primary attack goal | DLP, behavioral analytics, incident response |

---

## Contributing

Found an error or want to add more detail? Contributions are welcome!
Please open an **issue** or submit a **pull request**.

---
