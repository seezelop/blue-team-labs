LAB 02 - Wazuh Sysmon Process Monitoring
Objective

The objective of this lab is to detect and investigate Windows process execution events using Sysmon and Wazuh.

This lab demonstrates how endpoint process telemetry can be collected, forwarded to a SIEM, and analyzed from a SOC perspective.

Detection Flow
Process execution on Windows
Sysmon Event ID 1
Windows Event Log
Wazuh Agent
Wazuh Server
Wazuh Dashboard
SOC investigation
Lab Architecture

Due to local hardware limitations, the lab uses the physical Windows host as the monitored endpoint and a virtual machine as the Wazuh server.

Windows Endpoint
Wazuh Agent
Sysmon
Windows Event Logs

The endpoint telemetry is forwarded to the Wazuh Server VM.

Wazuh Server VM
Wazuh Manager
Wazuh Indexer
Wazuh Dashboard
Sanitized Lab References
Windows endpoint: WINDOWS-ENDPOINT
Wazuh server: WAZUH-SERVER
Wazuh server IP: <WAZUH_SERVER_IP>
Analyst user: analyst-user
Tools Used
Windows 10
Sysmon
Sysmon configuration file
Wazuh Agent
Wazuh Server
Wazuh Dashboard
Ubuntu Server
VirtualBox
PowerShell
Windows Event Viewer
Scenario

Sysmon was installed on the Windows endpoint to collect detailed process execution telemetry.

A benign process was launched from PowerShell to generate a controlled Sysmon event.

Main Process Chain
powershell.exe
calc.exe

This activity was manually generated in a controlled lab environment.

The goal was to verify that:

Sysmon generated a process creation event.
Wazuh Agent collected the Sysmon event.
Wazuh Server received and processed the event.
Wazuh Dashboard displayed the alert.
The event could be analyzed using SOC investigation fields.
Sysmon Installation

Sysmon was downloaded from Microsoft Sysinternals and extracted on the Windows endpoint.

A base Sysmon configuration file was used to improve process telemetry collection.

Sysmon Event Channel

Microsoft-Windows-Sysmon/Operational

Main Event Analyzed

Sysmon Event ID 1 - Process Create

Wazuh Agent Configuration

By default, Wazuh Agent did not collect Sysmon events from the Sysmon Operational channel.

To collect Sysmon telemetry, the following values were added to the Wazuh Agent configuration file:

Location: Microsoft-Windows-Sysmon/Operational
Log format: eventchannel

After updating the configuration, the Wazuh Agent service was restarted using:

Restart-Service WazuhSvc

The agent continued running and started forwarding Sysmon events to Wazuh.

Adding the Sysmon channel to ossec.conf is a collection configuration. It is not a detection rule.

Activity Generated

The following benign processes were executed to generate Sysmon telemetry:

Start-Process notepad.exe
Start-Process calc.exe
Main Event Selected

calc.exe launched from powershell.exe

This was safe and controlled lab activity.

Detection in Wazuh

Wazuh received the Sysmon event and generated an alert.

Wazuh Rule Information
Rule ID: 92066
Rule level: 4
Rule groups: sysmon, sysmon_eid1_detections, windows
MITRE ATT&CK ID: T1059.001
MITRE tactic: Execution
MITRE technique: PowerShell
Original Sysmon Event
Field: data.win.system.eventID
Value: 1

Sysmon Event ID 1 represents a process creation event.

Event Analysis
System Information
Event ID: 1
Provider name: Microsoft-Windows-Sysmon
Channel: Microsoft-Windows-Sysmon/Operational
Process Information
Image: C:\Windows\SysWOW64\calc.exe
Command line: "C:\Windows\system32\calc.exe"
Current directory: C:\Tools\Sysmon\
Parent Process Information
Parent image: C:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe
Parent command line: "C:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe"
Security Context
Integrity level: High
Wazuh Detection Information
Rule ID: 92066
Rule description: calc.exe binary in a suspicious location launched by powershell.exe
Field Interpretation
eventID: 1: Sysmon Process Create event.
providerName: Source that generated the event, in this case Microsoft-Windows-Sysmon.
channel: Windows Event Log channel where the event was recorded.
image: Process that was executed.
commandLine: Command line used to launch the process.
currentDirectory: Directory from which the process was launched.
parentImage: Process that launched the child process.
parentCommandLine: Command line associated with the parent process.
integrityLevel: Privilege and integrity context of the process.
rule.id: 92066: Wazuh rule that matched the event.
T1059.001: MITRE ATT&CK mapping associated with PowerShell.
SOC Investigation Notes
Observed Process Chain
powershell.exe
calc.exe

In this lab, the activity was expected and controlled.

However, from a SOC perspective, parent-child process relationships are important because they help analysts understand how a process was launched.

PowerShell launching another process is not automatically malicious. The activity must be evaluated in context.

Questions an Analyst Should Ask
What process was executed?
Who executed the process?
What command line was used?
What was the parent process?
Is this parent-child relationship expected?
Was the process launched from a normal path?
Was the activity repeated?
Is there related network activity?
Was there any follow-up behavior after process execution?
Investigation Assessment

The event was classified as expected lab activity because it was manually generated to validate Sysmon and Wazuh telemetry.

MITRE ATT&CK

Wazuh mapped the event to:

MITRE ATT&CK ID: T1059.001
Tactic: Execution
Technique: PowerShell

This mapping is useful because PowerShell is commonly used for legitimate administration but can also be abused by attackers.

In this lab, the mapping should be interpreted as detection context and not as proof of malicious activity.

The event shows PowerShell launching a child process, but there was no evidence of:

Malicious payload execution
Persistence
Lateral movement
Credential access
Data exfiltration
Command and control
Conclusion

This lab successfully demonstrated process execution monitoring using Sysmon and Wazuh.

The complete detection flow was validated:

PowerShell launched calc.exe.
Sysmon generated Event ID 1.
Wazuh Agent collected the Sysmon event.
Wazuh Server received and processed the event.
Wazuh generated Rule 92066.
Wazuh mapped the activity to MITRE ATT&CK T1059.001.
The event was reviewed from a SOC perspective.

The activity was confirmed as expected lab behavior because it was generated manually in a controlled environment.

Evidence and Screenshots
Sysmon Process Create - calc.exe

The following screenshot shows a Sysmon Event ID 1 generated when calc.exe was executed.

Insert screenshot here.

Parent Process Analysis

The event shows that calc.exe was launched by powershell.exe, which is useful for process tree analysis.

Insert screenshot here.

Sysmon Event ID 1 Details

The event was collected from the Microsoft-Windows-Sysmon/Operational channel.

Insert screenshot here.

Wazuh Rule and MITRE Mapping

Wazuh classified the event using Rule 92066 and mapped the activity to MITRE ATT&CK T1059.001 - PowerShell.

Insert screenshot here.

Security and Sanitization Notes

Before publishing screenshots or event data, sensitive information was removed or masked.

Sanitized Fields
Real hostname
Real username
Agent name
Agent IP
Manager name
Process GUIDs
Event Record IDs
Personal paths
Unnecessary identifiers
Public Documentation Placeholders
WINDOWS-ENDPOINT
WAZUH-SERVER
<WAZUH_SERVER_IP>
analyst-user
<REDACTED_GUID>
<REDACTED_HASH>
Lessons Learned
Sysmon provides detailed endpoint telemetry beyond default Windows logs.
Sysmon Event ID 1 represents process creation.
Wazuh can collect Sysmon events when the agent is configured to read the Sysmon Operational channel.
Wazuh Rule IDs and Sysmon Event IDs are different concepts.
Parent-child process relationships are important for SOC investigations.
PowerShell launching another process is not always malicious, but the activity should be reviewed in context.
MITRE ATT&CK mappings should be validated against the available evidence.
Telemetry must exist before a SIEM can detect or analyze activity.

By default, the Wazuh Agent collects common Windows channels such as:

Security
System
Application

However, detailed process creation telemetry requires an additional source such as Sysmon.

To collect Sysmon events, the agent must be configured to read the following channel:

Microsoft-Windows-Sysmon/Operational

Adding this channel to ossec.conf configures event collection. Detection rules are applied later by Wazuh after the event has been collected.

Possible Improvements
Generate additional benign process execution scenarios.
Compare normal parent-child process relationships with suspicious relationships.
Add PowerShell logging in a future lab.
Detect suspicious command-line arguments.
Create a custom Wazuh rule for specific process chains.
Add network telemetry to correlate process execution with outbound connections.
Extend the lab with controlled Living-off-the-Land scenarios.
Final Takeaway

Sysmon provides the telemetry, Wazuh provides the detection, and SOC analysis provides the context.

This lab validated the complete telemetry-to-investigation pipeline for Windows process execution monitoring.
