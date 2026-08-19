# Blue Team Labs

Practical Blue Team / SOC labs focused on detection, investigation, SIEM, Windows security, log analysis and security automation.

This repository documents my hands-on learning path after completing CompTIA Security+. The goal is to move from theory to practical defensive security skills by building labs, generating events, analyzing evidence and documenting the investigation process.

## Main Focus

This project focuses on:

* Windows Security Events
* Wazuh SIEM
* Log analysis
* Authentication monitoring
* SOC investigation workflow
* Sysmon
* PowerShell from a defensive perspective
* MITRE ATT&CK mapping
* File Integrity Monitoring
* SIEM detections
* Security automation
* SOAR concepts

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
```

The goal is not only to use tools, but to understand how security events are created, collected, detected and investigated.

## Current Lab Architecture

The initial architecture was adapted due to local hardware limitations.

```text
Windows Endpoint
Wazuh Agent
        |
        | Security events
        v
Wazuh Server VM
Ubuntu Server + Wazuh Manager + Wazuh Indexer + Wazuh Dashboard
```

Docker and additional tools may be introduced later for automation, SOAR, log generation and supporting services.

## Labs

| Lab                                                                        | Topic                                            | Status    | Main Tools                   |
| -------------------------------------------------------------------------- | ------------------------------------------------ | --------- | ---------------------------- |
| [01 - Wazuh Windows Authentication](labs/01-wazuh-windows-authentication/) | Failed Windows logon detection and investigation | Completed | Windows, Wazuh, Event Viewer |
| 02 - Wazuh Sysmon                                                          | Process execution and endpoint telemetry         | Planned   | Wazuh, Sysmon, Windows       |
| 03 - Suspicious Activity Detection                                         | Controlled suspicious activity analysis          | Planned   | Wazuh, Windows               |
| 04 - File Integrity Monitoring                                             | File creation and modification detection         | Planned   | Wazuh FIM                    |
| 05 - PowerShell Defensive Monitoring                                       | PowerShell activity from a defensive perspective | Planned   | Windows, PowerShell, Wazuh   |
| 06 - SOC Investigation Workflow                                            | Alert triage and evidence correlation            | Planned   | Wazuh                        |
| 07 - Splunk Basics                                                         | Log ingestion and SPL searches                   | Planned   | Splunk                       |
| 08 - Splunk Detections                                                     | Detection logic and searches                     | Planned   | Splunk                       |
| 09 - SOAR Basics                                                           | Alert enrichment and response workflow           | Planned   | SOAR, Python                 |
| 10 - Mini SOC Scenario                                                     | Integrated detection, investigation and response | Planned   | SIEM, SOAR, Python           |

## Completed Labs

### LAB 01 - Wazuh Windows Authentication

This lab focused on detecting failed Windows authentication attempts using Wazuh.

The lab demonstrated the following flow:

```text
Failed login attempt
→ Windows Security Event ID 4625
→ Wazuh Agent
→ Wazuh Server
→ Wazuh Rule 60122
→ Dashboard alert
→ SOC investigation
```

Key learning points:

* Windows Event ID `4625` represents a failed logon attempt.
* Wazuh applies its own rule ID to classify the event.
* Windows Event IDs and Wazuh Rule IDs are different concepts.
* Event fields such as username, logon type, status, substatus and source help with investigation.
* MITRE ATT&CK mappings should be reviewed critically and validated against the evidence.

Lab documentation:

[01 - Wazuh Windows Authentication](labs/01-wazuh-windows-authentication/)

## Security and Sanitization

Before publishing any evidence, screenshots or logs, sensitive information must be reviewed and sanitized.

Do not publish:

* Real hostnames
* Real usernames
* Passwords
* Tokens
* Personal paths
* Real SIDs
* Private information
* Browser profile information
* Unnecessary IP addresses
* Internal identifiers that are not required for the lab explanation

Sanitized placeholders should be used when needed:

```text
WINDOWS-ENDPOINT
WAZUH-SERVER
<WAZUH_SERVER_IP>
<REDACTED_SID>
<REDACTED_EVENT_RECORD_ID>
analyst-user
```

## Repository Structure

```text
blue-team-labs/
│
├── README.md
│
├── labs/
│   └── 01-wazuh-windows-authentication/
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
```

## Next Steps

Planned next labs:

1. Add Sysmon telemetry to Windows.
2. Detect and investigate process execution events.
3. Monitor file integrity changes.
4. Analyze PowerShell activity from a defensive perspective.
5. Start building simple automation workflows using Python.

## Disclaimer

All labs are performed in a controlled environment for defensive security learning purposes.

No malware, unauthorized access or attacks against external systems are used in this project.

