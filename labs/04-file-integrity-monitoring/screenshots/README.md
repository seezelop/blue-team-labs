# LAB 04 - File Integrity Monitoring

## Objective

Detect and investigate file creation, modification and deletion events in Windows using **Wazuh File Integrity Monitoring (FIM)**.

The lab focuses on understanding how Wazuh monitors file integrity in real time and how these events are analyzed from a SOC perspective.

---

## Lab Architecture

### Windows Endpoint

* Windows 10
* Wazuh Agent
* Wazuh FIM
* PowerShell

### Wazuh Server

* Wazuh Manager
* Wazuh Indexer
* Wazuh Dashboard
* Ubuntu Server VM

### Monitored Directory

`C:\SOC-Lab\FIM`

Configured with real-time monitoring.

---

## FIM Configuration

The following directory was added to the Wazuh Agent `syscheck` configuration:

`<directories realtime="yes">C:\SOC-Lab\FIM</directories>`

After updating `ossec.conf`, the Wazuh Agent service was restarted.

---

## Activity Generated

A test file was created inside the monitored directory:

`C:\SOC-Lab\FIM\test-fim.txt`

Three actions were performed:

* File creation
* File modification
* File deletion

All activity was manually generated in a controlled lab environment.

---

## Detection in Wazuh

### File Created

* **Rule ID:** `554`
* **Rule level:** `5`
* **Description:** `File added to the system.`
* **FIM event:** `added`

### File Modified

* **Rule ID:** `550`
* **Rule level:** `7`
* **Description:** `Integrity checksum changed.`
* **FIM event:** `modified`
* **MITRE:** `T1565.001 - Stored Data Manipulation`

Wazuh detected changes in:

* File size
* Modification time
* MD5
* SHA1
* SHA256

### File Deleted

* **Rule ID:** `553`
* **Rule level:** `7`
* **Description:** `File deleted.`
* **FIM event:** `deleted`
* **MITRE:** `T1070.004 - File Deletion`
* **MITRE:** `T1485 - Data Destruction`

---

## Event Analysis

Relevant FIM fields analyzed:

* `syscheck.path`
* `syscheck.event`
* `syscheck.mode`
* `size_before`
* `size_after`
* `md5_before`
* `md5_after`
* `sha1_before`
* `sha1_after`
* `sha256_before`
* `sha256_after`
* `rule.id`
* `rule.level`
* `rule.mitre`

The monitored file was detected in **real-time mode**.

---

## SOC Investigation Notes

FIM does not need to know whether a file change is malicious.

Its purpose is to detect that integrity changed and provide evidence for investigation.

In this lab:

* Creation generated Rule `554`.
* Modification generated Rule `550`.
* Deletion generated Rule `553`.

Wazuh detected changes using file metadata and hashes.

The exact file content before and after modification was not stored by the FIM event.

---

## MITRE ATT&CK

Observed mappings included:

* `T1565.001 - Stored Data Manipulation`
* `T1070.004 - File Deletion`
* `T1485 - Data Destruction`

These mappings provide investigation context but do not prove malicious activity.

All events in this lab were intentionally generated.

---

## Evidence and Screenshots

### FIM Dashboard

Overview of FIM alerts generated during the lab.

![Wazuh FIM dashboard](screenshots/dashboard.jpg)

### File Creation

Shows the creation of `test-fim.txt` detected by Wazuh Rule `554`.

![FIM file creation Rule 554](screenshots/01-fim-file-added-rule-554.JPG)

### File Modification

Shows the integrity change detected by Rule `550`, including before/after metadata and hashes.

![FIM file modification Rule 550](screenshots/02-fim-file-modified-rule-550.JPG)

### File Deletion and MITRE Mapping

Shows the deletion event detected by Rule `553` together with the associated MITRE ATT&CK mappings.

![FIM file deletion and MITRE mapping](screenshots/03-fim-file-deleted-mitre.JPG)

---

## Security and Sanitization Notes

Before publishing, sensitive information was removed or masked:

* Real hostname
* Username
* Agent name
* Agent IP
* Manager name
* SIDs
* GUIDs
* Personal paths
* Unnecessary hashes
* Event identifiers

---

## Lessons Learned

* Wazuh FIM can monitor directories in real time.
* Rule `554` detects file creation.
* Rule `550` detects integrity changes.
* Rule `553` detects file deletion.
* Hashes help prove that file content changed.
* FIM detects integrity changes without needing malware.
* MITRE mappings must be interpreted in context.
* File activity alone is not proof of compromise.

---

## Conclusion

LAB 04 validated the complete FIM workflow:

* File created → Rule `554`
* File modified → Rule `550`
* File deleted → Rule `553`

Wazuh successfully detected and recorded all changes in real time.

The main takeaway is that **File Integrity Monitoring provides visibility into unexpected file changes that may require SOC investigation**.
