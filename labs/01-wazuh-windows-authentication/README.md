# LAB 01 - Wazuh Windows Authentication

## Objective

The objective of this lab is to detect and investigate failed Windows authentication attempts using **Wazuh**.

This lab demonstrates a basic SOC workflow:

- Windows activity
- Windows Security event generation
- Wazuh Agent collection
- Wazuh Server processing
- Wazuh Dashboard alert
- SOC investigation

---

## Lab Architecture

Due to local hardware limitations, the lab was adapted to use the physical Windows host as the monitored endpoint and a virtual machine as the Wazuh server.

### Windows Endpoint

- Wazuh Agent installed
- Windows Security Events enabled
- Events and alerts forwarded to the Wazuh Server

### Wazuh Server VM

- Ubuntu Server
- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

### Sanitized Lab References

- **Windows endpoint:** `WINDOWS-ENDPOINT`
- **Wazuh server:** `WAZUH-SERVER`
- **Wazuh server IP:** `<WAZUH_SERVER_IP>`
- **Test user:** `soclabuser`
- **Analyst user:** `analyst-user`

---

## Tools Used

- Windows 10
- Wazuh Agent
- Wazuh Server
- Wazuh Dashboard
- Ubuntu Server
- VirtualBox
- Windows Event Viewer
- PowerShell

---

## Scenario

A local test user was created on the Windows endpoint. Multiple failed authentication attempts were generated using an incorrect password.

The goal was to verify that:

- Windows generated failed logon events.
- Wazuh Agent collected the events.
- Wazuh Server received and processed the events.
- Wazuh Dashboard displayed the alerts.
- The events could be investigated using relevant SOC analysis fields.

---

## Configuration Summary

The Wazuh Agent was installed on the Windows endpoint and configured to communicate with the Wazuh Server.

The agent appeared as active in the Wazuh Dashboard.

- **Agent name:** `WINDOWS-ENDPOINT`
- **Agent status:** `Active`
- **Manager:** `WAZUH-SERVER`

---

## Activity Generated

A local test user was created using:

- `net user soclabuser Lab12345! /add`

Failed authentication attempts were generated using:

- `runas /user:soclabuser cmd`

An incorrect password was entered several times to generate failed logon events.

> The credentials and activity used in this lab were created exclusively for a controlled local environment.

---

## Detection in Wazuh

Wazuh detected the failed logon attempts and generated alerts using the following rule:

- **Rule ID:** `60122`
- **Rule description:** `Logon Failure - Unknown user or bad password`
- **Rule level:** `5`
- **Rule groups:** `windows`, `windows_security`, `authentication_failed`

### Original Windows Event

- **Field:** `data.win.system.eventID`
- **Value:** `4625`

Windows Event ID `4625` represents a failed logon attempt.

---

## Event Analysis

### Windows Event Information

- **Event ID:** `4625`
- **Channel:** `Security`
- **Provider name:** `Microsoft-Windows-Security-Auditing`

### Authentication Information

- **Target username:** `soclabuser`
- **Logon type:** `2`
- **IP address:** `::1`
- **Status:** `0xc000006d`
- **Substatus:** `0xc000006a`

### Wazuh Detection Information

- **Rule ID:** `60122`
- **Rule description:** `Logon Failure - Unknown user or bad password`

---

## Field Interpretation

- **`eventID: 4625`:** Failed Windows logon event.
- **`targetUserName: soclabuser`:** Account associated with the failed authentication attempt.
- **`logonType: 2`:** Interactive logon attempt.
- **`ipAddress: ::1`:** IPv6 localhost address.
- **`status: 0xc000006d`:** Authentication failure.
- **`subStatus: 0xc000006a`:** Incorrect password.
- **`rule.id: 60122`:** Wazuh rule that classified the failed logon event.

---

## SOC Investigation Notes

Four failed logon events were observed during the lab.

The events had the same rule description and were associated with the same test user. Their timestamps showed repeated failed attempts within a short period.

In a real SOC environment, these events should be grouped and compared using fields such as:

- `targetUserName`
- Source IP address
- `workstationName`
- `logonType`
- `status`
- `subStatus`
- Timestamp
- `agent.name`

### Investigation Assessment

The activity was classified as **expected lab activity** because the failed authentication attempts were manually generated in a controlled environment.

---

## MITRE ATT&CK Consideration

Wazuh mapped the rule to:

- **MITRE ATT&CK ID:** `T1531`
- **Tactic:** `Impact`
- **Technique:** `Account Access Removal`

However, the evidence observed in this lab only confirms failed local authentication attempts. There was no evidence of account access removal or actual impact.

This highlights an important SOC lesson:

> Tool-generated MITRE ATT&CK mappings should be reviewed critically and validated against the actual evidence.

---

## Evidence and Screenshots

### Wazuh Failed Logon Alerts

The following screenshot shows multiple failed Windows logon alerts detected by Wazuh.

![Wazuh failed logon alerts](screenshots/wazuh-logon-failure-events-sanitized.png)

### Windows Event ID 4625 Details

The following screenshot shows the detailed event fields collected by Wazuh, including the original Windows Security Event ID `4625` and the target test user.

![Windows Event ID 4625 details](screenshots/event-details-4625-sanitized.png)

### Additional Recommended Evidence

- Wazuh Agent displayed as active in the dashboard
- Wazuh event list showing failed logon alerts
- Event details showing `eventID: 4625`
- Event details showing `targetUserName: soclabuser`
- Event details showing `rule.id: 60122`

---

## Security and Sanitization Notes

Before uploading screenshots or event data, sensitive information should be removed, masked, or blurred.

### Information to Sanitize

- Real hostname
- Real username
- Real SIDs
- Personal paths
- Private IP addresses when not required
- Tokens
- Passwords
- Browser profile information
- Unnecessary internal identifiers

### Public Documentation Placeholders

- `WINDOWS-ENDPOINT`
- `WAZUH-SERVER`
- `<WAZUH_SERVER_IP>`
- `<REDACTED_SID>`
- `<REDACTED_EVENT_RECORD_ID>`
- `analyst-user`

---

## Conclusion

This lab successfully demonstrated the detection and investigation of failed Windows authentication attempts using Wazuh.

The complete detection flow was validated:

- A failed login attempt was generated.
- Windows generated Security Event ID `4625`.
- Wazuh Agent collected the Windows event.
- Wazuh Server received and processed the event.
- Wazuh matched Rule `60122`.
- The alert appeared in the Wazuh Dashboard.
- The event was reviewed from a SOC perspective.

The event was confirmed as expected lab activity because it originated from a controlled test using a local test user.

---

## Lessons Learned

- Failed Windows logon attempts generate Security Event ID `4625`.
- Wazuh Agent can collect Windows Security events and forward them to Wazuh Server.
- Wazuh uses its own rule IDs to classify events.
- Windows Event IDs and Wazuh Rule IDs are different concepts.
- SOC analysts should review relevant event fields before reaching conclusions.
- Tool-generated MITRE ATT&CK mappings should be validated against the actual evidence.
- Repeated authentication failures should be analyzed using account, source, logon type, and timestamp context.

---

## Possible Improvements

- Repeat the lab using a dedicated Windows virtual machine.
- Add Sysmon telemetry in a future lab.
- Compare failed local logons with failed network logons.
- Create custom Wazuh rules for repeated failed logon attempts.
- Add Active Directory authentication scenarios in future labs.
- Automate alert enrichment using Python or SOAR concepts in later labs.

---

## Final Takeaway

> **Windows generates the authentication evidence, Wazuh detects and classifies the activity, and SOC analysis provides the context required to reach an accurate conclusion.**
