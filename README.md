# Blue Team Labs

Practical Blue Team / SOC labs focused on detection, investigation, SIEM, Windows security, log analysis and security automation.

This repository documents my hands-on learning path after completing CompTIA Security+. The goal is to move from theory to practical defensive security skills by building labs, generating events, analyzing evidence and documenting the investigation process.

## Main Focus

- Windows Security Events
- Wazuh SIEM
- Sysmon
- Log analysis
- SOC investigation workflow
- MITRE ATT&CK mapping
- PowerShell from a defensive perspective
- File Integrity Monitoring
- Security automation
- SOAR concepts

## Lab Approach

Each lab follows a practical workflow:

```text
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
├── Wazuh Manager
├── Wazuh Indexer
└── Wazuh Dashboard

Docker and additional tools may be introduced later for automation, SOAR, log generation and supporting services.

Labs
Lab	Topic	Status	Main Tools
01 - Wazuh Windows Authentication	Failed Windows logon detection and investigation	Completed	Windows, Wazuh, Event Viewer
02 - Wazuh Sysmon Process Monitoring	Process execution and endpoint telemetry	Completed	Wazuh, Sysmon, Windows
03 - Suspicious Activity Detection	Controlled suspicious activity analysis	Planned	Wazuh, Windows
04 - File Integrity Monitoring	File creation and modification detection	Planned	Wazuh FIM
05 - PowerShell Defensive Monitoring	PowerShell activity from a defensive perspective	Planned	Windows, PowerShell, Wazuh
06 - SOC Investigation Workflow	Alert triage and evidence correlation	Planned	Wazuh
07 - Splunk Basics	Log ingestion and SPL searches	Planned	Splunk
08 - Splunk Detections	Detection logic and searches	Planned	Splunk
09 - SOAR Basics	Alert enrichment and response workflow	Planned	SOAR, Python
10 - Mini SOC Scenario	Integrated detection, investigation and response	Planned	SIEM, SOAR, Python
Completed Labs
LAB 01 - Wazuh Windows Authentication

This lab focused on detecting failed Windows authentication attempts using Wazuh.

Detection flow:

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
Event fields such as username, logon type, status, substatus and source help with investigation.
MITRE ATT&CK mappings should be reviewed critically and validated against the evidence.

Documentation:

01 - Wazuh Windows Authentication

LAB 02 - Wazuh Sysmon Process Monitoring

This lab focused on detecting and investigating Windows process execution events using Sysmon and Wazuh.

Detection flow:

PowerShell launched calc.exe
→ Sysmon Event ID 1
→ Wazuh Agent
→ Wazuh Server
→ Wazuh Rule 92066
→ MITRE T1059.001
→ SOC investigation

Key learning points:

Sysmon Event ID 1 represents process creation.
Wazuh can collect Sysmon events when the agent reads the Sysmon Operational channel.
Parent-child process relationships are important for SOC investigations.
PowerShell launching another process is not always malicious, but it should be reviewed in context.
MITRE ATT&CK mappings should be validated against the actual evidence.

Documentation:

02 - Wazuh Sysmon Process Monitoring

Security and Sanitization

Before publishing any evidence, screenshots or logs, sensitive information must be reviewed and sanitized.

Do not publish:

Real hostnames
Real usernames
Passwords
Tokens
Personal paths
Real SIDs
Private information
Browser profile information
Unnecessary IP addresses
Internal identifiers that are not required for the lab explanation

Sanitized placeholders should be used when needed:

WINDOWS-ENDPOINT
WAZUH-SERVER
<WAZUH_SERVER_IP>
<REDACTED_SID>
<REDACTED_EVENT_RECORD_ID>
<REDACTED_GUID>
<REDACTED_HASH>
analyst-user
Repository Structure
blue-team-labs/
│
├── README.md
│
├── labs/
│   ├── 01-wazuh-windows-authentication/
│   │   ├── README.md
│   │   ├── screenshots/
│   │   ├── evidence/
│   │   └── notes/
│   │
│   └── 02-wazuh-sysmon-process-monitoring/
│       ├── README.md
│       ├── screenshots/
│       ├── evidence/
│       └── notes/
│
├── docs/
│   ├── lab-architecture.md
│   └── sanitization-checklist.md
│
└── scripts/
Next Steps

Planned next labs:

Analyze controlled suspicious activity.
Monitor file integrity changes.
Analyze PowerShell activity from a defensive perspective.
Practice SOC alert triage and evidence correlation.
Start building simple automation workflows using Python and SOAR concepts.
Disclaimer

All labs are performed in a controlled environment for defensive security learning purposes.

No malware, unauthorized access or attacks against external systems are used in this project.