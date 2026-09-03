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
* Wireshark
* Npcap
* OpenSSH Server used temporarily for controlled remote-access testing

### Wazuh Server

* Ubuntu Server VM
* Wazuh Manager
* Wazuh Indexer
* Wazuh Dashboard
* VirtualBox

### Kali Linux Endpoint

* Kali Linux
* Wazuh Agent
* auditd
* Python
* requests

Due to local hardware limitations, only the virtual machines required for each lab are kept running simultaneously.

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
* Logon Type
* Status and Substatus analysis

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

Observed process chain:

`powershell.exe → calc.exe`

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

**Status:** Completed

Monitoring PowerShell activity using Windows PowerShell Operational logs and Wazuh.

Main topics:

* PowerShell execution monitoring
* Script Block Logging
* Windows Event ID `4104`
* PowerShell Operational logs
* Wazuh event collection
* Telemetry vs alerting
* Detection troubleshooting

Successful detection:

* Wazuh Rule `91843`

Observed MITRE mappings:

* `T1059.001 - PowerShell`
* `T1112 - Modify Registry`

Main lesson:

**Telemetry ≠ Alert**

An event may reach the SIEM without generating an alert unless a detection rule matches the activity.

---

### LAB 06 - Network Analysis with Wireshark / tcpdump

**Status:** Completed

Basic network traffic analysis using Wireshark.

Traffic analyzed:

* DNS
* HTTP
* HTTPS
* TCP
* TLS

Main topics:

* Packet capture
* DNS queries and responses
* DNS A records
* Transaction IDs
* HTTP GET requests
* HTTP 200 responses
* TCP three-way handshake
* TLS Client Hello
* TLS Server Hello
* SNI
* Encrypted Application Data
* Basic network investigation

Main investigation flow:

**DNS → TCP → HTTP / TLS → Network Investigation**

---

### LAB 07 - Advanced File Integrity Monitoring

**Status:** Completed

Advanced Wazuh File Integrity Monitoring using Who-data.

The lab extended standard FIM by identifying not only that a file changed, but also which user and which process performed the modification.

Controlled modifications were performed using:

* `notepad.exe`
* `powershell.exe`

Main topics:

* Wazuh FIM
* `realtime="yes"`
* `whodata="yes"`
* User attribution
* Process attribution
* File hashes
* File metadata changes
* SOC investigation

Detection:

* Wazuh Rule `550`
* Level `7`
* `Integrity checksum changed`

MITRE ATT&CK:

* `T1565.001 - Stored Data Manipulation`
* Tactic: `Impact`

Main investigation flow:

**File Modification → FIM → Who-data → User + Process → Wazuh Rule 550 → MITRE ATT&CK**

---

### LAB 08 - Linux auditd with Wazuh

**Status:** Completed

Monitoring Linux file activity using auditd integrated with Wazuh.

Architecture:

`Kali Linux → auditd → audit.log → Wazuh Agent → Wazuh Manager`

Main topics:

* Linux audit events
* auditd
* `/var/log/audit/audit.log`
* Wazuh Agent
* Audit keys
* File write monitoring
* Process attribution
* User attribution
* SOC investigation

Controlled file:

`/opt/soc-lab/audit-test.txt`

Audit key:

`audit-wazuh-w`

Observed process:

`/usr/bin/tee`

Detection:

* Wazuh Rule `80781`
* Level `3`
* `Audit: Watch - Write access`

Main investigation flow:

**Linux Activity → auditd → audit.log → Wazuh Agent → Wazuh Manager → Rule 80781 → SOC Investigation**

---

### LAB 09 - Threat Intelligence / IOC Enrichment

**Status:** Completed

IOC enrichment using Python and the VirusTotal API.

Indicators analyzed:

* IP addresses
* File hashes

Main topics:

* Threat Intelligence
* Indicators of Compromise
* VirusTotal API
* Python
* API key handling
* JSON processing
* Reputation analysis
* Automated classification
* Analyst interpretation

Test cases included:

`1.1.1.1 → BENIGN`

EICAR test hash:

`44d88612fea8a8f36de82e1278abb02f`

Result:

`MALICIOUS`

The risk thresholds used in the Python script were created specifically for the laboratory and are not an official VirusTotal scoring system.

Main workflow:

**IOC → VirusTotal API → Reputation → Classification → Analyst Decision**

Main lesson:

**Threat Intelligence adds context, but does not replace analyst judgment.**

---

### LAB 10 - Simulated Lateral Movement Evidence

**Status:** Completed

Controlled remote access from Kali Linux to a Windows endpoint using SSH.

Architecture:

`Kali → SSH → Windows Endpoint → Windows Security / Sysmon → Wazuh`

Main topics:

* Remote authentication
* OpenSSH
* Windows Event ID `4624`
* Logon Type `3`
* Sysmon Event ID `1`
* Remote command execution
* Event correlation
* MITRE ATT&CK
* Wazuh investigation

Main Wazuh rules:

* Rule `60106` — Windows Logon Success
* Rule `92052` — Windows command prompt started by an abnormal process

Observed MITRE mappings:

* `T1078 - Valid Accounts`
* `T1059.003 - Windows Command Shell`

Main investigation flow:

**Kali → SSH → Windows Logon → sshd.exe → cmd.exe → Wazuh → SOC Investigation**

Main lesson:

**Lateral movement is not one alert. It is a sequence of correlated evidence.**

---

### LAB 11 - Active Directory Security Events

**Status:** Planned

Focus:

* Windows Server
* Active Directory Domain Services
* Authentication events
* User creation
* Failed and successful logons
* User and group changes
* Privileged group membership
* Security policies
* Windows Security Events
* Wazuh monitoring
* SOC investigation

The lab should use the lightest possible architecture due to local hardware limitations.

---

### LAB 12 - SOAR / Python Automation

**Status:** Planned

Focus:

* Alert automation
* Python
* Wazuh integrations
* n8n / SOAR
* Webhooks
* JSON
* IOC extraction
* Threat Intelligence
* VirusTotal
* Automated classification
* Notifications

Target workflow:

**Wazuh Alert → IOC Extraction → Threat Intelligence → Classification → Notification**

The first version should prioritize:

**Detect → Enrich → Notify**

Automated blocking should be treated as a later evolution.

---

### LAB 13 - Wazuh SOC Features

**Status:** Planned

Exploration of additional Wazuh capabilities that complement SOC monitoring and investigation.

Focus:

* Custom Rules
* Custom Decoders
* Syscollector
* Endpoint inventory
* Agent Groups
* Centralized configuration
* Registry monitoring with FIM
* Introduction to Active Response
* Other Wazuh features useful for SOC analysts

The objective is to improve:

* Detection
* Context
* Administration
* Response

Hardening-specific capabilities such as CIS Benchmarks and deep SCA analysis will be reserved for a separate hardening series.

---

### LAB 14 - Mini SOC Investigation Scenario

**Status:** Planned

Final integration scenario combining multiple sources of evidence from the series.

Possible evidence sources:

* Windows Security Events
* Sysmon
* Wazuh
* PowerShell
* File Integrity Monitoring
* Network evidence
* auditd
* Threat Intelligence
* Automation
* MITRE ATT&CK

Investigation objectives:

* Alert triage
* Timeline reconstruction
* Event correlation
* User identification
* Process analysis
* Host analysis
* IOC enrichment
* MITRE ATT&CK mapping
* SOC conclusion

The goal is to simulate a small end-to-end SOC investigation using techniques learned throughout the series.

---

## Security and Sanitization

Before publishing logs, screenshots or evidence, sensitive information is removed or replaced when necessary.

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
* API keys
* Passwords
* Unnecessary hashes
* MAC addresses
* Network interface identifiers

Public documentation may use placeholders such as:

* `WINDOWS-ENDPOINT`
* `WAZUH-SERVER`
* `<WAZUH_SERVER_IP>`
* `analyst-user`
* `<REDACTED_GUID>`
* `<REDACTED_HASH>`

Laboratory-only identifiers may remain visible when they do not reveal personal or sensitive information and contribute to the technical explanation.

---

## Learning Goals

This repository focuses on developing practical skills in:

* SOC investigation
* SIEM monitoring
* Windows Security Events
* Linux security events
* Sysmon
* Wazuh
* File Integrity Monitoring
* Wazuh Who-data
* PowerShell defensive monitoring
* Network analysis
* Linux auditd
* MITRE ATT&CK
* Threat Intelligence
* IOC enrichment
* Incident investigation
* Event correlation
* Remote access investigation
* Active Directory security monitoring
* Custom Wazuh rules
* Wazuh decoders
* Security automation
* SOAR
* Active Response
* Python for security

---

## Investigation Philosophy

The labs focus on understanding the complete defensive process rather than simply generating alerts.

Typical workflow:

**Activity → Telemetry → Detection → Investigation → Correlation → Context → Response**

An alert alone does not prove malicious activity.

The analyst must evaluate:

* User
* Process
* Parent process
* Host
* Source
* Destination
* Timestamp
* Authentication context
* Related events
* Threat Intelligence
* MITRE ATT&CK mapping

---

## Documentation

Each completed lab includes:

* Technical README
* Lab architecture
* Controlled activity
* Detection evidence
* Event analysis
* SOC investigation notes
* MITRE ATT&CK where applicable
* Sanitized screenshots
* Lessons learned
* Conclusion

The objective is to document not only **what Wazuh detected**, but also **how the evidence should be interpreted by a SOC analyst**.

---

## Progress

**Completed:** 10 / 14 labs

---

## Current Remaining Labs

* LAB 11 — Active Directory Security Events
* LAB 12 — SOAR / Python Automation
* LAB 13 — Wazuh SOC Features
* LAB 14 — Mini SOC Investigation Scenario

---

## Final Goal

The goal is not simply to generate security alerts, but to understand:

**what happened, how it was detected, what evidence supports the event and how an analyst should interpret it.**

---

## Disclaimer

All exercises are performed for educational and defensive purposes in controlled environments.

No malware is used and no external systems are targeted.
