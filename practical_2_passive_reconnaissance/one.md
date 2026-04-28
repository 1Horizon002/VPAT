# Experiment 02: Passive Reconnaissance

## 🎯 Aim
To implement passive reconnaissance techniques using tools: WHOIS, NSLOOKUP, DIG, theHarvester, and SHODAN in order to gather publicly available information about a target system.

---

## 📘 Part A: General Methodology & Theory

### 1. Theory for Oral Exam
* **Definition:** Passive reconnaissance is the initial phase of security testing where info is gathered **without direct interaction** (no packets sent to the target server).
* **Goal:** To map the attack surface using public records.
* **Sources:** DNS records, WHOIS databases, search engine caches, and IoT scanners.

### 2. Tool Overview
| Tool Name | Category | Primary Function |
| :--- | :--- | :--- |
| **WHOIS** | Domain Registry | Identifies registrar, owner, and expiration dates. |
| **NSLOOKUP** | DNS Utility | Queries basic IP and Mail Server (MX) records. |
| **DIG** | DNS Analysis | Provides detailed DNS headers, TTL, and Authority data. |
| **theHarvester** | OSINT | Scrapes emails, subdomains, and hostnames. |
| **SHODAN** | Network Search | Finds open ports, service banners, and device types. |

---

## 💻 Part B: Practical Execution (Commands & Examples)

Use these commands in your Kali Linux terminal. The targets used are from the experiment requirements.

### 1. WHOIS (Domain Information)
```bash
# Example target: vulnweb.com
whois testphp.vulnweb.com
```

### 2. NSLOOKUP (Name Server Lookup)
```bash
# Example: Find Mail Servers for vesit.ves.ac.in
nslookup -type=mx vesit.ves.ac.in
```

### 3. DIG (Domain Information Groper)
```bash
# Advanced DNS query for example.com
dig example.com ANY

# Querying specific MX records for testphp.vulnweb.com
dig testphp.vulnweb.com MX
```

### 4. theHarvester (OSINT Collection)
```bash
# Gathering data for testphp.vulnweb.com using Google
theHarvester -d testphp.vulnweb.com -l 200 -b google
```

### 5. SHODAN (IoT/Infrastructure Search)
1.  Open `https://www.shodan.io` in your browser.
2.  Search: `hostname:testphp.vulnweb.com`
3.  **Observation:** Check for open ports (e.g., 80, 443) and server headers (e.g., Nginx, Apache).

---

## 📊 Comparison & Sensitivity Analysis

| Target | Tool | Info Collected | Security Sensitivity |
| :--- | :--- | :--- | :--- |
| **testphp.vulnweb.com** | theHarvester | Emails, Subdomains | **High** |
| **testphp.vulnweb.com** | SHODAN | Open Ports, Banners | **High** |
| **example.com** | WHOIS | Registry Details | **Low** |
| **example.com** | DIG | Basic DNS Records | **Low** |

---

## ❓ Oral (Viva) Questions & Answers

**Q1: What makes a domain look "genuine" in a WHOIS record?**
> **Ans:** A legitimate domain usually has a recognized registrar (like GoDaddy or Namecheap), a registration period longer than one year, and valid organizational contact details.

**Q2: What is "TTL" in a DIG command output?**
> **Ans:** TTL stands for **Time To Live**. It tells the DNS resolver how long to cache the query result before asking the name server for an update.

**Q3: Why is SHODAN more dangerous than standard search engines?**
> **Ans:** Unlike Google, which indexes web content, Shodan indexes **devices** and **services**. It can reveal misconfigured industrial controllers, cameras, or servers with outdated software versions.

---

## ✅ Conclusion
This experiment demonstrates that targets like `testphp.vulnweb.com` reveal significant technical data (high sensitivity), whereas reserved domains like `example.com` show minimal exposure. Understanding these exposures is the first step in identifying a system's vulnerability.