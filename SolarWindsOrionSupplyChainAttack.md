# 🛰️ SolarWinds Orion Supply Chain Attack — Malware & DLL Reverse Engineering Analysis

![Threat Actor - APT29](https://img.shields.io/badge/Threat_Actor-APT29_(Cozy_Bear)-red)
![CVE](https://img.shields.io/badge/CVE-2020--10148-critical)
![Skill - Malware Analysis](https://img.shields.io/badge/Skill-Malware_Analysis-purple)
![Skill - Reverse Engineering](https://img.shields.io/badge/Skill-Reverse_Engineering-blue)
![Category - Supply Chain](https://img.shields.io/badge/Category-Supply_Chain_Attack-orange)

> **Objective:** Analyse the SolarWinds Orion supply-chain compromise (Sunburst malware), examine malicious .NET DLL content using decompilation tools, identify obfuscation and encoded malicious logic, and evaluate the kill-switch, C2 behaviour, and internal functionality of the malware.

---

## 📌 Overview

The **SolarWinds Orion compromise** was a high-impact supply chain attack attributed to **Russian APT29 (Cozy Bear)**. Attackers injected malicious code into legitimate SolarWinds Orion software updates between 2019–2020.

- **CVE:** 2020-10148  
- **CVSS:** 9.8  
- **Affected versions:** Orion Platform **2019.4 → 2020.2.1 HF1**  
- **Impact:** Creation of a backdoor, C2 communication, reconnaissance, persistence, security tool evasion  
- **Attack type:** Digitally-signed malicious DLL distributed through SolarWinds update infrastructure  

---

## 📅 Attack Timeline

| Date | Event |
|------|-------|
| **Sept 2019** | SolarWinds development environment breached |
| **Oct 2019** | First code injection tests performed |
| **Feb 2020** | Sunburst malware injected into Orion DLL |
| **Mar 2020** | Digitally-signed malicious update shipped to customers |
| **Dec 2020 – Feb 2021** | Industry response, kill-switch deployment, DNS sinkholing begins |
| **Apr 29 2022** | GoDaddy transfers control of avsvmcloud.com to Microsoft-led coalition, turning it into a global sinkhole |

---

## 🧠 Why It Avoided Detection

- **Digitally signed** — malware distributed as a trusted SolarWinds update  
- **Dormant for ~12–14 days** — reducing behavioural detection  
- **Obfuscated code** — strings compressed, key logic hashed or concealed  
- **Disabled local security controls** once activated  
- **Legitimate-looking C2 traffic** — used `avsvmcloud[.]com` domain  
- **Wildcard DNS records** — all subdomains resolved to same IP for victim profiling  
- **C2 hosts rotated across multiple U.S. ISPs** to appear legitimate  

---

## 🌐 C2 Infrastructure (Sunburst)

The malware contacted C2 subdomains that were **hash-like values derived from victim system data**, enabling tailored attacker instructions.

### C2 Host Rotation Examples  
(All historical known values from the investigation)

| Date | C2 IP | Location | Hosting Provider |
|------|-------|----------|------------------|
| 12/12/19 | `107.161.23.204` | Atlanta | RamNode |
| 12/12/19 | `192.161.187.200` | Atlanta | QuadraNet |
| 12/12/19 | `209.141.38.71` | Las Vegas | FranTech |
| 26/12/19 – 19/02/20 | Several | Various | GoDaddy |
| 01/10/20 | `13.65.251.83` | San Antonio | Microsoft |

### 2022 Kill-switch  
- Domain **avsvmcloud[.]com** taken over by Microsoft et al.  
- Converted into a **malware sinkhole** to neutralise ongoing beaconing  

---

## 🧩 Malware Analysis: Sunburst DLL (BusinessLayer)

### Tools Used
- **JetBrains dotPeek** – .NET decompiler  
- **Deflate decompression tool** – to decode compressed strings  
- **Manual static analysis** using opcode, string resolution, and control-flow interpretation  

### Malicious Component
Located in:

SolarWinds.Orion.Core.BusinessLayer
→ OrionImprovementBusinessLayer



---

## 🔍 Key Sections of Reverse-Engineered Code

### ### 🔹 Line 40 — Hash Table of Registry Keys
The malware stores **hashed registry key names**.  
It hashes victim registry values and **compares them to this internal list** to know which security features to disable.

---

### 🔹 Code Search: `gethash`
Searching for `"gethash"` reveals the **custom hashing algorithm** used to compare system attributes and determine target readiness (anti-analysis, security settings).

---

### 🔹 Lines 420–434 — Compressed Strings (Not Hashes)
Initially appear as hash-like strings, but actually **compressed payloads**.

Using a deflate decompression tool reveals **active reconnaissance logic**, including:

- Network adapter list  
- MAC Addresses  
- DHCP Enabled  
- DHCP Server / Hostname  
- DNS Domain Search Order  
- DNS Servers  
- IP Address  
- Subnet  
- Default Gateway  

This is **environment mapping**, used to profile the victim.

---

### 🔹 Line 451 — OS Information Query
Compressed string decodes to reconnaissance targeting:

- Operating system version  
- Build number  
- Environment variables  

---

### 🔹 Dormancy Timers (Lines 270–292)
Key logic controlling the **initial 12–14 day sleep cycle**.

Example decoded value:

- `"17291806236368054941UL"` — hash of the malware host executable:  
  `solarwinds.businesslayerhost.exe`

- `288, 366` → The number of hours to remain dormant (~12 to 14 days)

---

### 🔹 Registry Permission Hijacking  
- **Lines 1132–1158** – Takes **ownership** of registry keys  
- **Lines 1170–1191** – Modifies **permissions** to disable or tamper with system protections  

---

### 🔹 Security Tool Detection (Lines 1458–1508)
Compares registry strings to hashed values in its internal list.  
Targets various:

- AV products  
- EDR solutions  
- Security configurations  

---

### 🔹 C2 Beacon Logic (Lines 2287–2320)
- Contacts C2 via `avsvmcloud[.]com`  
- Sends victim profiling data  
- Receives **encoded command block**  
- Decodes and executes instructions from attackers  

---

### 🔹 Command Set (Lines 2460–2586)
Decoded commands indicate capabilities including:

- Network enumeration  
- File system access  
- Process interaction  
- Disabling system services  
- Exfiltration preparation  
- C2 re-tasking  

---

## 🧾 Skills Demonstrated

- Malware reverse engineering  
- .NET DLL decompilation & static analysis  
- Identifying obfuscation / compressed strings  
- Decompression and interpretation of malicious logic  
- Understanding of supply-chain attack techniques  
- Knowledge of C2 infrastructure and evasion behaviour  
- CVE analysis and threat-actor attribution  
- Registry and OS-level defensive bypass understanding  

---

## 🚀 Future Enhancements & Additional Write-Ups

To elevate this project further, consider adding:

### 🧪 **Dynamic Analysis Lab (Behavioural Testing)**
- Execute the DLL in a sandbox (e.g., **FlareVM**, **REMnux**, **Cuckoo**)  
- Capture:
  - Network traffic (PCAP)  
  - Registry modifications  
  - File system changes  
  - Process creation events  

### 📊 **Malware Mapping**
- Create a **MITRE ATT&CK technique map**, showing each observed behaviour  
- Include:
  - T1027 (Obfuscation)  
  - T1059 (Execution)  
  - T1562 (Defence Evasion)  
  - T1041 (C2 Exfiltration)

### 🔐 **YARA Rules**
Write and include a custom **YARA detection rule** for the malicious DLL based on unique strings or hashes.

### 🛰️ **DNS Sinkhole Analysis**
- Visualise C2 traffic  
- Explain the industry kill-switch  
- Show how defenders monitor callbacks  

### 📝 **Incident Response Report**
Create a professional IR report summarising:
- Indicators of compromise (IoCs)  
- Timeline of intrusion  
- Recommended mitigations  
- Lessons learned  

### 🛡️ **Blue Team Countermeasures**
Add defensive recommendations such as:

- Blocking C2 domains  
- Endpoint monitoring for DLL tampering  
- Registry/key permission monitoring  
- Supply-chain verification procedures  
- Memory scanning for compressed strings  

### ⚙️ **Automation Scripts**
Add small scripts for:
- Extracting compressed strings  
- Hash detection tests  
- IOC extraction  

---

## 🧑‍💻 Author
**[Michael Bruce]** — Aspiring Cybersecurity Practitioner  
Malware Analysis | Threat Intelligence | Defensive Security  
GitHub: https://github.com/Vavain  
Linkedin: https://www.linkedin.com/in/michaelbruce90/

---

## ⚠️ Disclaimer
This analysis is for **educational and defensive purposes only**.  
All work was performed in a controlled environment on non-production systems.  
Do not analyse or execute malware on any system you do not fully control.



