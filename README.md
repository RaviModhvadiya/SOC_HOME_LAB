# SOC Home Lab

![SOC Lab](https://img.shields.io/badge/SIEM-Splunk-black?style=for-the-badge&logo=splunk)
![Kali](https://img.shields.io/badge/Attacker-Kali_Linux-557C94?style=for-the-badge&logo=kalilinux)
![Windows](https://img.shields.io/badge/Victim-Windows_10-0078D6?style=for-the-badge&logo=windows)
![VirtualBox](https://img.shields.io/badge/Platform-VirtualBox-183A61?style=for-the-badge&logo=virtualbox)

---

## 📌 Overview
A fully functional **Security Operations Center (SOC) Home Lab** 
built to simulate real-world cyberattacks and practice 
threat detection, log analysis, and incident response 
using industry-standard tools.

---

## 🖥️ Lab Architecture


---

## 🛠️ Tools & Technologies

| Category | Tool | Purpose |
|---|---|---|
| SIEM | Splunk Enterprise | Log collection, analysis & alerting |
| Log Forwarding | Splunk Universal Forwarder | Forward victim logs to Splunk |
| Virtualization | Oracle VirtualBox | Run VMs on host machine |
| Logging | Sysmon | Advanced Windows event logging |
| Reconnaissance | Nmap | Network & port scanning |
| Brute Force | Hydra | Password attacks |
| Exploitation | Metasploit | Vulnerability exploitation |

---

## ⚙️ Lab Setup

### 🔵 Host Machine — Splunk SIEM
- Splunk Enterprise installed on Windows 11
- Configured to receive logs on port 9997
- Created indexes for Windows Event Logs
- Host-Only network adapter configured

### 🔴 SOC-LAB — Victim Machine (Windows 10 VM)
- Splunk Universal Forwarder installed
- outputs.conf configured to forward logs to Host
- inputs.conf configured for Security, System & Application logs
- Process Auditing enabled via auditpol
- Connected via Host-Only VirtualBox network

### ⚔️ ATTACKER — Kali Linux VM
- Fresh Kali Linux installation
- Host-Only network adapter
- All attack tools pre-installed
- Connected to same isolated network as SOC-LAB

---

## 🔍 Attack Scenarios & Detections

### ⚔️ Scenario 1 — Network Reconnaissance (Nmap)

**Attack on Kali:**
```bash
nmap -sV -O <target-ip>
```

**Open Ports Discovered on Victim:**
135/tcp → Microsoft Windows RPC
139/tcp → NetBIOS
445/tcp → SMB

**Splunk Detection Query:**
index=main host="SOC-LAB" EventCode=4688 
| stats count by Account_Name Creator_Process_Name 
| sort -count

**Result:** Process creation events logged ✅

---

### ⚔️ Scenario 2 — Brute Force Attack (Hydra)

**Attack on Kali:**
```bash
hydra -l administrator -P /usr/share/wordlists/rockyou.txt smb://<target-ip>
```

**Splunk Detection Query:**
index=main host="SOC-LAB" EventCode=4625
| stats count by Account_Name
| sort -count

**EventCode 4625 = Failed Login Attempt** ✅

---

### ⚔️ Scenario 3 — Command Execution Tracking

**Splunk Detection Query:**
index=main host="SOC-LAB" EventCode=4688
Creator_Process_Name="C:\Windows\System32\cmd.exe"
| table _time Account_Name Creator_Process_Name
| sort -_time

**Result:** Every CMD session recorded with timestamp & user ✅

---

## 📊 Key Splunk SPL Reference

| EventCode | Meaning | Use Case |
|---|---|---|
| 4624 | Successful Login | Track access |
| 4625 | Failed Login | Detect brute force |
| 4688 | Process Created | Detect malware/tools |
| 4648 | Login with Credentials | Lateral movement |
| 7036 | Service State Change | Detect persistence |

---

## 📸 Screenshots

### Splunk Receiving Logs from SOC-LAB
<img width="750" height="750" alt="image" src="https://github.com/user-attachments/assets/4c401020-5062-4ef4-a5b0-00044ab92338" />

### Nmap Scan from Kali
<img width="650" height="550" alt="image" src="https://github.com/user-attachments/assets/62375f31-a8d3-42f1-8e49-948a846dc0be" />


### Splunk Detection Results
*(Add screenshot)*

### CMD Activity Detection
*(Add screenshot)*

---

## 🎯 Skills Demonstrated

- ✅ SIEM Setup & Configuration (Splunk Enterprise)
- ✅ Log Forwarding & Collection (Splunk Universal Forwarder)
- ✅ Virtual Network Design & Configuration
- ✅ Attack Simulation (Nmap, Hydra, Metasploit)
- ✅ Threat Detection using SPL Queries
- ✅ Windows Event Log Analysis
- ✅ Incident Investigation Workflow
- ✅ Process Auditing Configuration

---

## 🗺️ Future Improvements

- [ ] Install Sysmon for advanced logging
- [ ] Configure Splunk Alerts for real-time detection
- [ ] Add Metasploit exploitation scenarios
- [ ] Map detections to MITRE ATT&CK Framework
- [ ] Add Active Directory VM for domain attacks

---

## 📚 References

- [Splunk Documentation](https://docs.splunk.com)
- [MITRE ATT&CK Framework](https://attack.mitre.org)
- [TryHackMe SOC Path](https://tryhackme.com)
- [Sysmon Config - SwiftOnSecurity](https://github.com/SwiftOnSecurity/sysmon-config)
- [VirtualBox Documentation](https://www.virtualbox.org/manual)

---

> ⚠️ **Disclaimer:** This lab is built purely for 
> educational purposes in an isolated virtual environment. 
> All attacks are performed only within this private lab.
