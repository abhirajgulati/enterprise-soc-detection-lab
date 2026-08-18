# Enterprise SOC Detection Lab

A hands-on cybersecurity project where I built a small enterprise Active Directory environment, configured centralized security monitoring with Splunk and Sysmon, simulated common Active Directory attacks, and investigated the resulting telemetry from a SOC analyst perspective.

> **Scope:** All attack simulations were performed in an isolated lab environment created and controlled by me for educational and defensive security purposes.

---

## Project Overview

The goal of this project was to gain practical experience with both sides of a SOC investigation: generating realistic security activity and then identifying the evidence it leaves behind.

I built an `enterprise.local` Active Directory environment, implemented users, groups, OUs, Group Policies and role-based file access, then configured Windows auditing, Sysmon and Splunk to collect endpoint and authentication telemetry.

Once monitoring was working, I used Kali Linux to simulate several Active Directory attack techniques and investigated the resulting events in Splunk.

### Project Workflow

**BUILD → HARDEN → MONITOR → SIMULATE → DETECT → INVESTIGATE → ANALYSE**

---

## Lab Architecture

| System | Purpose |
|---|---|
| **DC01** | Windows Server 2022 Domain Controller |
| **WIN11-01** | Domain-joined Windows 11 workstation |
| **KALI01** | Attack simulation and enumeration |
| **Splunk** | Centralized log collection and investigation |
| **Sysmon** | Enhanced endpoint telemetry |
| **BloodHound** | Active Directory relationship and attack-path analysis |

**Domain:** `enterprise.local`

The environment included departmental OUs for **IT, HR and Finance**, security groups, test users, departmental file shares and Group Policy controls.

---
## Full Project Report

A condensed portfolio report documenting the complete lab build, security monitoring configuration, attack simulations, SOC investigations, and BloodHound analysis is available below.

➡️ **[View the Enterprise SOC Detection Lab Report](docs/Enterprise_SOC_Lab.pdf)**

> Detailed step-by-step implementation notes and troubleshooting evidence are maintained separately from the public portfolio.

## Technologies & Skills

- Active Directory Domain Services
- Windows Server 2022
- Windows 11
- Group Policy
- Role-Based Access Control (RBAC)
- Windows Security Auditing
- Splunk
- Splunk Universal Forwarder
- Sysmon
- Kali Linux
- BloodHound
- Kerberos
- SMB / LDAP enumeration
- MITRE ATT&CK
- Security event investigation
- SPL querying

---

## Security Monitoring

Windows Security logs from the domain controller and workstation were forwarded into Splunk.

Sysmon was also deployed to provide additional endpoint visibility including:

- Process creation
- Network connections
- DNS activity
- Service state changes

I validated the monitoring pipeline before performing attack simulations to make sure the expected telemetry was reaching Splunk.

---

## Attack Simulations & Detection

### 1. Password Spraying

A controlled password-spraying simulation was performed against multiple domain accounts from Kali Linux.

In Splunk, I investigated:

- **Event ID 4625** — Failed logon
- **Event ID 4624** — Successful logon
- Repeated authentication failures across multiple accounts
- Common source IP activity
- Successful authentication following multiple failures

This demonstrated how a SOC analyst can correlate authentication events rather than relying on a single failed login.

---

### 2. Kerberoasting

A service account with an SPN was created in the lab to simulate Kerberoasting activity.

The investigation focused on:

- **Event ID 4769** — Kerberos Service Ticket Request
- Service account ticket requests
- Requesting user
- Source system
- Kerberos encryption information

This demonstrated how Kerberos ticket activity can provide evidence of potential service-account targeting.

---

### 3. AS-REP Roasting

A controlled account was configured without Kerberos pre-authentication to reproduce an AS-REP roasting scenario.

The resulting authentication activity was reviewed to understand how insecure account configuration can expose Kerberos material to offline password attacks.

---

### 4. SMB & LDAP Enumeration

Kali Linux was used to perform controlled reconnaissance against the Active Directory environment.

The activity included:

- SMB enumeration
- Domain information discovery
- LDAP queries
- User and group discovery
- Network service reconnaissance

The exercise helped connect attacker reconnaissance activity with the telemetry available to defenders.

---

## BloodHound Analysis

BloodHound was used to collect and visualize Active Directory relationships within the lab.

The analysis included:

- Users
- Groups
- Computers
- Organizational relationships
- Privilege relationships
- Potential paths toward privileged groups

This added an identity-security perspective to the project by showing how Active Directory relationships can be analysed beyond individual Windows events.

---

## Detection Summary

| Activity | Primary Telemetry | Investigation Goal |
|---|---|---|
| Password Spraying | 4625 / 4624 | Multiple authentication failures followed by possible success |
| Kerberoasting | 4769 | Suspicious Kerberos service-ticket requests |
| AS-REP Roasting | Kerberos authentication events | Accounts without pre-authentication |
| SMB Enumeration | Windows/Sysmon telemetry | Reconnaissance against domain systems |
| LDAP Enumeration | Directory/network telemetry | Active Directory discovery |
| BloodHound | AD relationship data | Identify potentially risky privilege relationships |

---

## MITRE ATT&CK Alignment

The lab provided practical exposure to techniques associated with:

- **T1110 – Brute Force**
- **T1558.003 – Kerberoasting**
- **T1558.004 – AS-REP Roasting**
- **T1087 – Account Discovery**
- **T1069 – Permission Groups Discovery**
- **T1018 – Remote System Discovery**

MITRE ATT&CK was used as a framework to connect simulated attacker behaviour with defensive detection and investigation.

---

## Key Lessons Learned

One of the biggest things I learned from this project was that performing an attack was only half of the exercise. I spent just as much time making sure the correct telemetry was being collected and understanding what evidence each action left behind.

Troubleshooting also became an important part of the project. I encountered issues involving Group Policy, Splunk configuration and log forwarding, as well as BloodHound setup. Working through these problems helped me understand how the individual components of a monitoring environment depend on each other.

Most importantly, the project taught me not to treat a single security event as the complete story. Correlating users, source systems, authentication failures, successful logons and Kerberos activity provided much more useful context during investigations.

---

## Project Documentation

A detailed portfolio report containing the lab build, configuration, attack simulations, Splunk investigations and supporting screenshots is included in this repository.

**Full report:** `docs/Enterprise-SOC-Lab-Portfolio-Report.pdf`

---

## Future Improvements

I plan to continue developing the lab by exploring:

- Additional Splunk detection rules and alerts
- Improved dashboards and correlation searches
- Additional Windows attack simulations
- Microsoft Sentinel / SIEM exposure
- Further MITRE ATT&CK mapping

---

## Author

**Abhiraj Gulati**  
Cybersecurity Student | Melbourne, Australia

Interested in SOC operations, security analysis, detection engineering, network security and defensive cybersecurity.
