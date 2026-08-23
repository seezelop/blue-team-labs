# Blue Team Labs

Practical Blue Team / SOC laboratory series focused on defensive security, detection engineering and security event investigation.

The objective of this repository is to move from theory to hands-on SOC analysis using controlled Windows and Linux environments.

All activities are performed in a safe lab environment without malware or attacks against external systems.

---

## Current Lab Environment

### Windows Endpoint

* Windows 10
* Wazuh Agent
* Sysmon
* Windows Event Logs
* PowerShell

### Wazuh Server

* Ubuntu Server VM
* Wazuh Manager
* Wazuh Indexer
* Wazuh Dashboard
* VirtualBox

Due to local hardware limitations, the Windows physical host is used as the monitored endpoint while the Wazuh infrastructure runs inside a virtual machine.

---

## Repository Structure

* `labs/` — Individual Blue Team labs
* `docs/` — General documentation
* `scripts/` — Automation and analysis scripts

Each lab contains its own documentation, evidence and sanitized screenshots.

---

## Lab Roadmap

### LAB 01 - Wazuh Windows Authentication

**Status:** Completed

Detection and investigation of failed Windows authentication attempts.

Main topics:

* Windows Security Events
* Event ID `4625`
* Wazuh Rule `60122`
* Authentication investigation

---

### LAB 02 - Wazuh Sysmon Process Monitoring

**Status:** Completed

Monitoring Windows process execution using Sysmon and Wazuh.

Main topics:

* Sysmon Event ID `1`
* Process creation
* PowerShell
* Parent-child process relationships
* Wazuh Rule `92066`
* MITRE ATT&CK `T1059.001`

---

### LAB 03 - Suspicious Process Investigation

**Status:** Completed

Investigation of legitimate Windows commands that may become suspicious depending on context.

Commands analyzed:

* `whoami`
* `hostname`
* `ipconfig`

Main process chains:

* `powershell.exe → cmd.exe → whoami.exe`
* `powershell.exe → cmd.exe → hostname.exe`
* `powershell.exe → cmd.exe → ipconfig.exe`

Main Wazuh rules:

* Rule `92004`
* Rule `92032`

Main topics:

* Process investigation
* Parent-child correlation
* Command-line analysis
* MITRE ATT&CK validation
* Context-based SOC analysis

---

### LAB 04 - File Integrity Monitoring

**Status:** Completed

Detection of file creation, modification and deletion using Wazuh File Integrity Monitoring.

Monitored activity:

* File creation → Rule `554`
* File modification → Rule `550`
* File deletion → Rule `553`

Main topics:

* Wazuh FIM
* Real-time directory monitoring
* File hashes
* Integrity changes
* MITRE ATT&CK
* File activity investigation

Observed MITRE mappings included:

* `T1565.001 - Stored Data Manipulation`
* `T1070.004 - File Deletion`
* `T1485 - Data Destruction`

---

### LAB 05 - PowerShell Defensive Monitoring

**Status:** Planned

Focus:

* PowerShell execution monitoring
* Command-line analysis
* Windows Event Logs
* Sysmon
* Wazuh
* Legitimate vs suspicious PowerShell activity

---

### LAB 06 - Network Analysis with Wireshark / tcpdump

**Status:** Planned

Focus:

* Packet capture
* DNS
* HTTP/HTTPS
* Outbound connections
* Basic network investigation

---

### LAB 07 - Advanced File Integrity Monitoring

**Status:** Planned

Focus:

* Sensitive file monitoring
* Normal vs suspicious file activity
* Advanced Wazuh FIM analysis

---

### LAB 08 - Linux auditd with Wazuh

**Status:** Planned

Focus:

* Linux audit events
* Commands
* Users
* Security-relevant system changes
* Wazuh integration

---

### LAB 09 - Threat Intelligence / IOC Enrichment

**Status:** Planned

Focus:

* IP addresses
* Domains
* Hashes
* Threat Intelligence APIs
* Python
* IOC enrichment

---

### LAB 10 - Simulated Lateral Movement Evidence

**Status:** Planned

Focus:

* Controlled lateral movement indicators
* Windows telemetry
* Sysmon
* Wazuh
* Process and event correlation

No real attacks or malware will be used.

---

### LAB 11 - Active Directory Security Events

**Status:** Planned

Focus:

* Authentication events
* Users and groups
* Security policies
* Windows Server
* Active Directory
* Wazuh

---

### LAB 12 - SOAR / Python Automation

**Status:** Planned

Focus:

* Alert automation
* Python
* Wazuh integrations
* n8n / SOAR
* IOC enrichment
* Automated response workflows

---

### LAB 13 - Mini SOC Investigation Scenario

**Status:** Planned

Final integration scenario combining multiple sources of evidence.

Focus:

* Alert investigation
* Timeline reconstruction
* Event correlation
* Sysmon
* Wazuh
* Windows telemetry
* Automation where applicable

---

## Security and Sanitization

Before publishing logs, screenshots or evidence, sensitive information is removed or replaced.

Examples include:

* Hostnames
* Usernames
* Agent names
* IP addresses
* Manager names
* SIDs
* GUIDs
* Event Record IDs
* Personal paths
* Tokens
* Passwords
* Unnecessary hashes

Public documentation uses placeholders such as:

* `WINDOWS-ENDPOINT`
* `WAZUH-SERVER`
* `<WAZUH_SERVER_IP>`
* `analyst-user`
* `<REDACTED_GUID>`
* `<REDACTED_HASH>`

---

## Learning Goals

This repository focuses on developing practical skills in:

* SOC investigation
* SIEM monitoring
* Windows Security Events
* Sysmon
* Wazuh
* File Integrity Monitoring
* PowerShell defensive monitoring
* Network analysis
* Linux auditing
* MITRE ATT&CK
* Threat Intelligence
* Incident investigation
* Security automation
* SOAR

---

## Progress

**Completed:** 4 / 13 labs

---

## Final Goal

Build a practical Blue Team / SOC portfolio demonstrating the complete defensive workflow:

**Telemetry → Detection → Investigation → Correlation → Response → Automation**
