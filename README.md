# Blue Team Labs

Practical Blue Team and SOC labs focused on detection, investigation, SIEM, Windows security, log analysis, and security automation.

This repository documents my hands-on learning path after completing **CompTIA Security+**. The goal is to move from theory to practical defensive security skills by building labs, generating events, analyzing evidence, and documenting the investigation process.

---

## Main Focus

- Windows Security Events
- Wazuh SIEM
- Sysmon
- Log analysis
- SOC investigation workflows
- MITRE ATT&CK mapping
- PowerShell from a defensive perspective
- File Integrity Monitoring
- Security automation
- SOAR concepts

---

## Lab Approach

Each lab follows a practical workflow:

- Activity generation
- Endpoint telemetry
- Log collection
- SIEM detection
- Event investigation
- Evidence analysis
- SOC conclusion
- Documentation

The goal is not only to use security tools, but also to understand how events are created, collected, detected, and investigated.

---

## Current Lab Architecture

The initial architecture was adapted due to local hardware limitations.

### Windows Endpoint

- Wazuh Agent
- Sysmon
- Windows Event Logs

Security telemetry is forwarded from the Windows endpoint to the Wazuh Server VM.

### Wazuh Server VM

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

Docker and additional tools may be introduced later for automation, SOAR, log generation, and supporting services.

---

## Labs

### Completed

- **Lab 01 — Wazuh Windows Authentication**  
  Failed Windows logon detection and investigation.  
  **Tools:** Windows, Wazuh, and Event Viewer.  
  **Documentation:** [View Lab 01](labs/01-wazuh-windows-authentication/README.md)

- **Lab 02 — Wazuh Sysmon Process Monitoring**  
  Process execution monitoring and endpoint telemetry analysis.  
  **Tools:** Wazuh, Sysmon, Windows, PowerShell, and Event Viewer.  
  **Documentation:** [View Lab 02](labs/02-wazuh-sysmon-process-monitoring/README.md)

### Planned

- **Lab 03 — Suspicious Activity Detection**  
  Controlled suspicious activity analysis using Wazuh and Windows.

- **Lab 04 — File Integrity Monitoring**  
  File creation and modification detection using Wazuh FIM.

- **Lab 05 — PowerShell Defensive Monitoring**  
  PowerShell activity analysis from a defensive perspective.

- **Lab 06 — SOC Investigation Workflow**  
  Alert triage and evidence correlation using Wazuh.

- **Lab 07 — Splunk Basics**  
  Log ingestion and foundational SPL searches.

- **Lab 08 — Splunk Detections**  
  Detection logic and investigation searches using Splunk.

- **Lab 09 — SOAR Basics**  
  Alert enrichment and response workflows using SOAR concepts and Python.

- **Lab 10 — Mini SOC Scenario**  
  Integrated detection, investigation, automation, and response scenario.

---

## Completed Labs

### Lab 01 — Wazuh Windows Authentication

This lab focused on detecting and investigating failed Windows authentication attempts using Wazuh.

#### Detection Flow

- Failed login attempt
- Windows Security Event ID `4625`
- Wazuh Agent collection
- Wazuh Server processing
- Wazuh Rule `60122`
- Dashboard alert
- SOC investigation

#### Key Learning Points

- Windows Event ID `4625` represents a failed logon attempt.
- Wazuh applies its own rule ID to classify the event.
- Windows Event IDs and Wazuh Rule IDs are different concepts.
- Fields such as username, logon type, status, substatus, and source help with event investigation.
- MITRE ATT&CK mappings should be reviewed critically and validated against the available evidence.

#### Documentation

- [Lab 01 — Wazuh Windows Authentication](labs/01-wazuh-windows-authentication/README.md)

---

### Lab 02 — Wazuh Sysmon Process Monitoring

**Status: Completed**

This lab focused on detecting and investigating Windows process execution events using Sysmon and Wazuh.

#### Detection Flow

- PowerShell launched `calc.exe`
- Sysmon generated Event ID `1`
- Wazuh Agent collected the event
- Wazuh Server processed the event
- Wazuh matched Rule `92066`
- Wazuh mapped the event to MITRE ATT&CK `T1059.001`
- The event was reviewed from a SOC perspective

#### Key Learning Points

- Sysmon Event ID `1` represents process creation.
- Wazuh can collect Sysmon events when the agent reads the Sysmon Operational channel.
- Parent-child process relationships are important for SOC investigations.
- PowerShell launching another process is not automatically malicious, but the behavior should be reviewed in context.
- MITRE ATT&CK mappings should be validated against the actual evidence.
- Adding the Sysmon channel to `ossec.conf` configures event collection; it does not create a detection rule.

#### Documentation

- [Lab 02 — Wazuh Sysmon Process Monitoring](labs/02-wazuh-sysmon-process-monitoring/README.md)

---

## Security and Sanitization

Before publishing evidence, screenshots, or logs, sensitive information must be reviewed and sanitized.

### Do Not Publish

- Real hostnames
- Real usernames
- Passwords
- Tokens
- Personal paths
- Real SIDs
- Private information
- Browser profile information
- Unnecessary IP addresses
- Internal identifiers that are not required for the lab explanation

### Sanitized Placeholders

- `WINDOWS-ENDPOINT`
- `WAZUH-SERVER`
- `<WAZUH_SERVER_IP>`
- `<REDACTED_SID>`
- `<REDACTED_EVENT_RECORD_ID>`
- `<REDACTED_GUID>`
- `<REDACTED_HASH>`
- `analyst-user`

---

## Repository Structure

- `README.md` — Main repository documentation
- `labs/` — Individual lab documentation
  - `01-wazuh-windows-authentication/`
    - `README.md`
    - `screenshots/`
    - `evidence/`
    - `notes/`
  - `02-wazuh-sysmon-process-monitoring/`
    - `README.md`
    - `screenshots/`
    - `evidence/`
    - `notes/`
- `docs/` — Supporting documentation
  - `lab-architecture.md`
  - `sanitization-checklist.md`
- `scripts/` — Future security automation scripts

---

## Next Steps

- Analyze controlled suspicious activity.
- Monitor file integrity changes.
- Analyze PowerShell activity from a defensive perspective.
- Practice SOC alert triage and evidence correlation.
- Build simple automation workflows using Python and SOAR concepts.
- Continue documenting findings, evidence, and investigation conclusions.

---

## Disclaimer

All labs are performed in a controlled environment for defensive security learning purposes.

No malware, unauthorized access, or attacks against external systems are used in this project.
