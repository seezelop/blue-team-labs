### LAB 08 - Linux auditd with Wazuh

**Status:** Completed

Monitoring Linux file activity using auditd integrated with Wazuh.

Main topics:

* Linux audit events
* auditd
* `/var/log/audit/audit.log`
* Wazuh Agent
* Audit keys
* File write monitoring
* Process and user attribution
* Wazuh Rule `80781`
* SOC investigation

Main investigation flow:

**Linux Activity → auditd → audit.log → Wazuh Agent → Wazuh Manager → Rule 80781**

---

### LAB 09 - Threat Intelligence / IOC Enrichment

**Status:** Completed

IOC enrichment using Python and the VirusTotal API.

Indicators analyzed:

* IP addresses
* File hashes

Main topics:

* Threat Intelligence
* IOC enrichment
* VirusTotal API
* Python
* API key handling
* Reputation analysis
* Automated classification
* Analyst interpretation

Test cases included:

* Public IP → `BENIGN`
* EICAR test hash → `MALICIOUS`

Main workflow:

**IOC → VirusTotal API → Reputation → Classification → Analyst Decision**

---

### LAB 10 - Simulated Lateral Movement

**Status:** Completed

Controlled remote access from Kali Linux to a Windows endpoint using SSH.

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

---

## Progress

**Completed:** 10 / 14 labs
