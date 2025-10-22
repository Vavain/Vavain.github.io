---
layout: default
---

## Welcome to another page

# 🧲 Honeypot Deployment and Attack Simulation

![Static Badge](https://img.shields.io/badge/Platform-Windows_11-blue)  
![Static Badge](https://img.shields.io/badge/Attack_Box-Kali_Linux-red)  
![Static Badge](https://img.shields.io/badge/Tool-Valhalla_Honeypot-orange)  
![Static Badge](https://img.shields.io/badge/Skill-Network_Security-green)  
![Static Badge](https://img.shields.io/badge/Status-Active_Project-brightgreen)

> **Objective:** Deploy and configure a honeypot to detect, log, and analyse unauthorised access attempts, demonstrating both offensive and defensive cybersecurity skills.

---

## 📑 Table of Contents
- [Overview](#-overview)
- [Objectives](#-objectives)
- [Tools and Technologies](#-tools-and-technologies)
- [Deployment Steps](#-deployment-steps)
- [Attack Simulation](#-attack-simulation)
- [Key Findings](#-key-findings)
- [Skills Demonstrated](#-skills-demonstrated)
- [Future Enhancements](#-future-enhancements)
- [Usage Instructions](#-usage-instructions)
- [Project Roadmap](#-project-roadmap)
- [Author](#-author)

---

## 🧭 Overview
This project demonstrates the deployment of a honeypot environment designed to emulate a vulnerable system. It uses **:contentReference[oaicite:0]{index=0}** on a Windows 11 host to log and monitor unauthorised activity. Attack traffic was simulated using **:contentReference[oaicite:1]{index=1}**, and captured interactions were analysed for patterns and insights.

---

## 🎯 Objectives
- Deploy a honeypot to mimic a production-like environment  
- Configure commonly targeted services to attract attackers  
- Simulate attacks to test detection capabilities  
- Log and analyse captured credentials and interactions

---

## 🧰 Tools and Technologies
- **Honeypot:** :contentReference[oaicite:2]{index=2}  
- **Host OS:** :contentReference[oaicite:3]{index=3}  
- **Attack Box:** :contentReference[oaicite:4]{index=4}  
- **Scanner:** :contentReference[oaicite:5]{index=5}

---

## 🏗️ Deployment Steps

### 1. Initial Setup
- Installed Valhalla Honeypot on Windows 11 host  
- Configured through built-in server settings

### 2. Web Server
- Enabled the web server
- <img width="906" height="434" alt="image" src="https://github.com/user-attachments/assets/83b1066f-655d-4a71-ae8a-c6e431b952bf" />

- Created `wwwroot` folder
- <img width="1677" height="914" alt="image" src="https://github.com/user-attachments/assets/9b33e54e-134e-4b88-a43e-b86b2fae6886" />

- Added `index.htm` page to mimic a landing page
- <img width="1679" height="911" alt="image" src="https://github.com/user-attachments/assets/48b6dfef-5431-4742-be52-a18f2d81bbca" />


### 3. Service Emulation
Enabled the following to simulate a realistic target:
- FINGER
- <img width="1065" height="529" alt="image" src="https://github.com/user-attachments/assets/77864ed5-26e1-4035-91d6-0cb2533f4b5c" />

- POP3
- <img width="966" height="514" alt="image" src="https://github.com/user-attachments/assets/ccb5230b-66c3-44bc-abf8-dc1ad26bd26a" />

- FTP (port 21, `admin / Password`)
- <img width="985" height="603" alt="image" src="https://github.com/user-attachments/assets/c31c6e25-e22f-46c2-9f46-2650698c1022" />

- SMTP (port 25)
- <img width="1158" height="605" alt="image" src="https://github.com/user-attachments/assets/f6c7a3c7-1081-46ba-bfa1-89d478721c1c" />

- Telnet (port 23, `admin / Password`)
- <img width="981" height="651" alt="image" src="https://github.com/user-attachments/assets/8373152e-4d17-43c5-a22f-d91af050558d" />


### 4. Activation
- Started monitoring mode to log all interactions

---

## 🧪 Attack Simulation

### 1. Reconnaissance
- No ICMP response (disabled)
- <img width="1841" height="761" alt="image" src="https://github.com/user-attachments/assets/c111c717-0203-46d3-97d3-d830698f7c0d" />

- Port scan with Nmap revealed exposed services
- <img width="918" height="873" alt="image" src="https://github.com/user-attachments/assets/9343cfb8-1e58-4bf1-b98c-7c3e786bb7d2" />


### 2. Access Attempts
- **HTTP:** Page and honeypot interface visible
- <img width="1816" height="775" alt="image" src="https://github.com/user-attachments/assets/a5c2902f-f4fa-43d1-97f8-4c1413d606ce" />

- **FTP:** `dir` commands captured and logged
- <img width="1737" height="733" alt="image" src="https://github.com/user-attachments/assets/977f0c95-8ddd-4486-82b7-896f9991276d" />

- **Telnet:** Cleartext credentials recorded
- <img width="1870" height="819" alt="image" src="https://github.com/user-attachments/assets/e3a7d8fa-3955-4593-97a4-56e73c1ef753" />


---

## 🧠 Key Findings
- Cleartext credential capture demonstrates low-security protocols  
- Logs show detailed command interaction  
- Simple service exposure is effective for collecting attacker telemetry

---

## 🛡️ Skills Demonstrated
- Network service configuration (HTTP, FTP, Telnet, SMTP, POP3)  
- Honeypot deployment and configuration  
- Attack simulation and enumeration using Kali Linux  
- Basic network reconnaissance with Nmap  
- Log analysis and security monitoring

---

## 🚀 Future Enhancements

### 1. Advanced Logging
- Integrate with **:contentReference[oaicite:6]{index=6}**, **:contentReference[oaicite:7]{index=7}**, or **:contentReference[oaicite:8]{index=8}**  
- Automate parsing and enrichment of honeypot logs  
- Build dashboards for attack trends and geolocation

### 2. Threat Intelligence Integration
- Cross-reference attacker IPs with open threat feeds  
- Geolocate and enrich metadata for reporting

### 3. Deception Enhancements
- Add realistic banners, fake admin panels, dummy credentials  
- Deploy multi-layered decoys to increase attacker dwell time

### 4. Protocol Expansion
- Add SSH, RDP, SMB services  
- Compare Valhalla logs with **:contentReference[oaicite:9]{index=9}** deployments

### 5. Cloud & Perimeter Deployment
- Deploy honeypot on cloud infrastructure for real-world telemetry  
- Correlate and visualise global attack activity

### 6. Incident Response Simulation
- Develop playbooks to react to honeypot detections  
- Automate alerting and reporting

### 7. Automation and Reporting
- Python or PowerShell scripts for daily reporting  
- Automated log pulls and dashboards published to GitHub

---

## 🧭 Usage Instructions

### Prerequisites
- Windows 11 host system  
- Administrative privileges  
- Optional: Kali Linux VM or physical system for testing

### Deployment
```powershell
# Example steps
1. Install Valhalla Honeypot
2. Enable web server and create wwwroot
3. Configure services (FTP, Telnet, SMTP, POP3)
4. Start monitoring mode










[back](./)
