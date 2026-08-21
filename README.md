Blue Team Labs

Practical Blue Team / SOC labs focused on detection, investigation, SIEM, Windows security, Sysmon, log analysis and security automation.

This repository documents my hands-on learning path after completing CompTIA Security+. The goal is to build practical defensive security skills by generating events, analyzing evidence and documenting SOC investigation workflows.

Focus Areas

Windows Security Events

Wazuh SIEM

Sysmon telemetry

Log analysis

SOC investigation workflow

MITRE ATT&CK mapping

PowerShell defensive monitoring

File Integrity Monitoring

Network analysis

Threat Intelligence

Basic DFIR concepts

Security automation and SOAR

Lab Approach

Each lab follows a practical workflow:

Activity generated
→ Endpoint telemetry
→ Log collection
→ SIEM detection
→ Event investigation
→ Evidence analysis
→ SOC conclusion
→ Documentation

The goal is not only to use tools, but to understand how security events are created, collected, detected and investigated.

Current Lab Architecture

The initial architecture was adapted due to local hardware limitations.

Windows Endpoint
├── Wazuh Agent
├── Sysmon
└── Windows Event Logs
        |
        | Security telemetry
        v
Wazuh Server VM
├── Ubuntu Server
├── Wazuh Manager
├── Wazuh Indexer
└── Wazuh Dashboard

Docker and additional tools may be introduced later for automation, SOAR, log generation and supporting services.

Labs

Lab

Topic

Status

Main Tools

01 - Wazuh Windows Authentication

Failed Windows logon detection and investigation

Completed

Windows, Wazuh, Event Viewer

02 - Wazuh Sysmon Process Monitoring

Process execution and endpoint telemetry

Completed

Windows, Sysmon, Wazuh

03 - Suspicious Process Investigation

Controlled suspicious process analysis

Planned

Windows, Sysmon, Wazuh

04 - Network Analysis

Basic traffic analysis with Wireshark / tcpdump

Planned

Wireshark, tcpdump

05 - File Integrity Monitoring

File creation and modification detection

Planned

Wazuh FIM

06 - PowerShell Defensive Monitoring

PowerShell activity from a defensive perspective

Planned

Windows, PowerShell, Wazuh

07 - Threat Intelligence / IOC Enrichment

Basic enrichment of indicators

Planned

Python, TI sources

08 - Basic Disk Forensics

Introductory disk artifact analysis

Planned

DFIR tools

09 - Splunk Basics

Log ingestion and SPL searches

Planned

Splunk

10 - SOAR / Python Automation

Alert enrichment and response workflow

Planned

SOAR, Python

11 - Active Directory Security Events

AD-focused security event analysis

Planned

Windows Server, AD

12 - Mini SOC Scenario

Integrated detection, investigation and response

Planned

SIEM, SOAR, Python

Completed Labs

LAB 01 - Wazuh Windows Authentication

Detected failed Windows authentication attempts using Wazuh.

Failed login attempt
→ Windows Security Event ID 4625
→ Wazuh Agent
→ Wazuh Server
→ Wazuh Rule 60122
→ Dashboard alert
→ SOC investigation

Key learning points:

Windows Event ID 4625 represents a failed logon attempt.

Wazuh applies its own rule ID to classify the event.

Windows Event IDs and Wazuh Rule IDs are different concepts.

Authentication events should be analyzed using username, source, logon type, status and timestamp.

Lab documentation:

01 - Wazuh Windows Authentication

LAB 02 - Wazuh Sysmon Process Monitoring

Detected Windows process execution events using Sysmon and Wazuh.

PowerShell launched calc.exe
→ Sysmon Event ID 1
→ Wazuh Agent
→ Wazuh Server
→ Wazuh Rule 92066
→ MITRE T1059.001
→ SOC investigation

Key learning points:

Sysmon Event ID 1 represents process creation.

Sysmon provides process details such as image, command line, parent process and integrity level.

Parent-child process relationships are important for SOC investigations.

PowerShell activity should be reviewed in context before making conclusions.

Lab documentation:

02 - Wazuh Sysmon Process Monitoring

Security and Sanitization

Before publishing evidence, screenshots or logs, sensitive information must be reviewed and sanitized.

Do not publish:

Real hostnames

Real usernames

Passwords or tokens

Personal paths

Real SIDs

Browser profile information

Unnecessary IP addresses

Internal identifiers not required for the lab explanation

Use placeholders when needed:

WINDOWS-ENDPOINT
WAZUH-SERVER
<WAZUH_SERVER_IP>
<REDACTED_SID>
<REDACTED_EVENT_RECORD_ID>
analyst-user

Repository Structure

blue-team-labs/
├── README.md
├── labs/
│   ├── 01-wazuh-windows-authentication/
│   └── 02-wazuh-sysmon-process-monitoring/
├── docs/
└── scripts/

Disclaimer

All labs are performed in a controlled environment for defensive security learning purposes.

No malware, unauthorized access or attacks against external systems are used in this project.