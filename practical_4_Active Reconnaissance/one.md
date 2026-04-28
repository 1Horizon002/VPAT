

---

# Practical No. 04: Active Reconnaissance

## 🎯 Aim
To perform active reconnaissance on a target system by directly interacting with it using network and web scanning tools. This includes documenting open ports, services, versions, OS hints, and web server vulnerabilities.

---

## 📖 Theory for Oral Exams
* **Active Reconnaissance:** The process of collecting information by directly sending packets to the target.
* **Key Characteristic:** Unlike passive recon, this generates network traffic and is likely to be detected by Firewalls or IDS (Intrusion Detection Systems).
* **Goal:** To create a "profile" of the target's attack surface (Open ports $\rightarrow$ Services $\rightarrow$ Versions $\rightarrow$ Vulnerabilities).

---

## 🆘 The "Help" Vault (Accessing All Commands)
If you need to find a specific flag or see all possible options for a tool during the lab, use these commands:

| Tool | Quick Help | Full Manual | Description |
| :--- | :--- | :--- | :--- |
| **Nmap** | `nmap -h` | `man nmap` | The "Bible" of network scanning. |
| **Netcat** | `nc -h` | `man nc` | The "Swiss Army Knife" of networking. |
| **Telnet** | `telnet` (then type `help`) | `man telnet` | Used for basic service interaction. |
| **WhatWeb** | `whatweb --help` | `man whatweb` | Next-gen web scanner for fingerprinting. |
| **Nikto** | `nikto -Help` | `man nikto` | Comprehensive web server scanner. |

---

## 💻 Practical Steps & Commands

### 1. Host Discovery
Confirm the target is alive before scanning.
```bash
ping <target_ip>
nmap -sn <target_ip>  # Ping scan (no port scan)
```

### 2. Port Scanning (Nmap)
* **TCP Connect Scan:** `nmap -sT <target_ip>` (Completes handshake)
* **Stealth (SYN) Scan:** `nmap -sS <target_ip>` (Half-open scan)
* **UDP Scan:** `nmap -sU <target_ip>` (For DNS/DHCP/SNMP)

### 3. Intelligence Gathering
* **Service Versioning:** `nmap -sV <target_ip>`
* **OS Detection:** `nmap -O <target_ip>`
* **Aggressive Scan:** `nmap -A <target_ip>` (OS, Version, Scripts, Traceroute)

### 4. Banner Grabbing & Fingerprinting
* **Netcat (Port 80):**
    ```bash
    nc <target_ip> 80
    GET / HTTP/1.1
    Host: <target_domain>
    ```
* **WhatWeb (Web Technologies):**
    ```bash
    whatweb http://<target_ip>
    ```

### 5. Vulnerability Scanning
* **Nikto:**
    ```bash
    nikto -h http://<target_ip>
    ```

---

## ❓ Oral Exam (Viva) Preparation

**Q1: What is the difference between `nmap -sT` and `nmap -sS`?**
* **Ans:** `-sT` (Connect scan) performs a full three-way handshake and is easily logged. `-sS` (SYN scan) is "stealthy" because it sends a RST (Reset) after receiving a SYN/ACK, never completing the connection.

**Q2: What is "Banner Grabbing"?**
* **Ans:** It is a technique used to identify the version of a service (like Apache or SSH) by reading the welcome message (the "banner") it sends when you connect to its port.

**Q3: Why would an Nmap scan show a port as "Filtered"?**
* **Ans:** It means Nmap cannot determine if the port is open or closed because a **Firewall** or network filter is blocking the probes.

---

## ✅ Conclusion
Active reconnaissance provides a foundation for the "Exploitation" phase. By identifying specific versions (e.g., *Apache 2.4.49*), an attacker can then search for specific CVEs (Common Vulnerabilities and Exposures) to gain access.

---

**Final Exam Tip:** If the terminal hangs while using `nc` or `telnet`, use `Ctrl + C` to break out. If you get a "Connection Refused," it usually means the service isn't running or a firewall is blocking you. 

Everything clear for tomorrow, or do you want to run through one more "what if" scenario for the oral exam? 😈