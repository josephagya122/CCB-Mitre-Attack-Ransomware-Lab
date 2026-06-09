# Standalone Security Lab: MITRE ATT&CK Ransomware Threat Simulation
**Target Enterprise:** Cash Capital Bank  
**Framework Focus:** MITRE ATT&CK v14, ISO 27001:2022, Detection Engineering  
**Author:** Joseph Agyapong  

## 📌 Lab Overview & Objective
This standalone technical lab simulates a dual-extortion ransomware attack (styled after active threat groups like LockBit 3.0 and Akira) targeting a hybrid financial enterprise network. 

The goal of this lab is to move beyond passive security compliance by mapping an active threat lifecycle to the **MITRE ATT&CK Framework**, developing proactive **Sigma detection logic**, and engineering an **Information Security Management System (ISMS)** defense baseline.

### 📊 Real-World Financial Sector Risk Factors:
* **Financial Blast Radius:** Average financial industry data breach costs hover around **$5.9 million** due to strict regulatory penalties and intense forensic timelines.
* **Operational Impact:** Domain-wide ransomware deployment causes an average of **16-22 days of operational downtime**, introducing immediate liquidity and reputational hazards.

---

## 🗺️ The Threat Model: Operation "Capital Lock"
* **Adversary Tactics:** Initial Access via Spearphishing $\rightarrow$ LSASS Credential Dumping $\rightarrow$ Lateral Movement via SMB $\rightarrow$ Data Exfiltration $\rightarrow$ Automated Inhibition of System Recovery.
* **The Extortion Scheme:** Exfiltration of 450 GB of sensitive consumer loan PII via encrypted channels, followed by network-wide data encryption.

### MITRE ATT&CK Execution Matrix

| Tactic | Technique ID | Technique Name | Lab Execution / Adversary Action |
| :--- | :--- | :--- | :--- |
| **Initial Access** | T1566.001 | Spearphishing Attachment | Delivery of an inbound email with a macro-enabled file named `Q2_Credit_Risk_Assessment.xlsm`. |
| **Execution** | T1059.001 | PowerShell Scripting | User opens the file; an obfuscated PowerShell script executes a payload payload download from an external server. |
| **Persistence** | T1543.003 | Windows Service Modification | The payload installs a malicious, hidden local Windows system service named `WinUpdateSvc`. |
| **Defense Evasion**| T1562.001 | Impair Defenses: Disable Tools | Script attempts to disable local Windows Defender real-time protection and clears event logs via `wevtutil`. |
| **Credential Access**| T1003.001 | OS Credential Dumping: LSASS | Rename and execution of Mimikatz binary to harvest cleartext Domain Admin tokens from memory. |
| **Discovery** | T1482 | Domain Trust Discovery | Network queries mapped to find Active Directory Domain Controllers, backup targets, and core SQL databases. |
| **Lateral Movement**| T1021.002 | SMB/Windows Admin Shares | Lateral movement across the internal subnet via administrative shares (`C$`) using compromised Admin tokens. |
| **Command & Control**| T1071.001 | Application Layer Protocol: Web | Command communication disguised inside standard outbound HTTPS (Port 443) traffic to bypass firewalls. |
| **Exfiltration** | T1048.002 | Exfiltration Over Cloud Protocol | Exfiltration of 450 GB of customer financial data directly to an external encrypted Mega.nz cloud bucket. |
| **Impact** | T1486 | Data Encrypted for Impact | Script triggers `vssadmin delete shadows /all /quiet`, killing system restore capabilities, before running the encryption routine. |

---

## 🛠️ Combined Defense Engineering (SecOps & GRC)

This lab addresses mitigation across two critical corporate functions:

### 1. Technical Detection & Engineering (SecOps)
* **Endpoint Hardening:** Implementation of an AD-wide Group Policy Object (GPO) to prevent standard users from holding local administrative tokens, effectively stopping LSASS memory injection.
* **Network Micro-segmentation:** Firewall policies explicitly restricting lateral SMB traffic (Port 445) between user workstation subnets.
* **Detection Engineering:** Deployment of centralized logging targeting abnormal script interpreters spawning from productivity applications (See the [`detection/`](./detection/) directory for engineering rules).

### 2. Governance, Risk, & Compliance (GRC)
* **ISO 27001 Mapping:** This lab directly validates and tests the enforcement of **Control A.8.20** (Network Security), **Control A.8.2** (Privileged Access Rights), and **Control A.5.29** (Information Security during Disruption).
* **Immutable Fail-safes:** Mandating automated, hourly block-level snapshot backups pushed to an isolated, read-only air-gapped cloud repository.
* **Incident Playbook:** Formalized containment and escalation procedures managed by the Incident Response Team (See the [`grc-playbooks/`](./grc-playbooks/) directory).

---
*Disclaimer: This repository consists of conceptual threat-modeling blueprints, defensive templates, and detection engineering logic built strictly for educational portfolio display. No production banking code, intellectual property, or live infrastructure configurations are contained herein.*
