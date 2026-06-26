## Incident Report: 
* Severity: Critical
* Status: Detected, Analyzed, Contained and Resolved

## Incident Summary
* On June 23, 2026, a multi-stage attack was conducted against the Active Directory domain controller (192.168.10.10) originating from 192.168.10.29. The attacker connected via RDP (method of credential aquisition unknown), established a session as Administrator, created and gave privileges to a backdoor account named "IT Admin," and executed Mimikatz to perform an attack dumping all domain credentials, including the krbtgt hash. The attack was detected via Wazuh alerts for suspicious account activity. Mimikatz was discovered during follow-up investigation of the domain controller. Full compromise is assumed due to exposure of the krbtgt hash.

## Date and Time
* Backdoor account created/enabled: June 23, 2026 at 1:34 AM
* Backdoor account granted privilages: June 23, 2026 at 1:37 AM
* Mimikatz executed: June 23, 2026 at 1:43 AM
* Detected by Wazuh: June 23, 2026 at 1:34 AM (account creation alert), June 23, 2026 at 1:37 AM (account privilege grant alert)
* Reviewed and contained: June 23, 2026 at 1:52 AM

## Attack Details
* Attack type: Backdoor Account Creation, Privilege Escalation, Credential Harvesting
* Tools used: RDP, Mimikatz
* Source IP: 192.168.10.29 (LAN)
* Destination IP: 192.168.10.10 (Domain Controller, LAB\DC01)
* Target: Active Directory domain — lab.local
* MITRE ATT&CK:
* T1078 — Valid Accounts (RDP access as Administrator)
* T1136.002 — Create Account: Domain Account (backdoor account)
* T1098 — Account Manipulation (added to Domain Admins)
* T1003.006 — OS Credential Dumping: DCSync (Mimikatz)

## Detection Details
Tool: Wazuh SIEM monitoring Windows Security event logs via agent on DC01
Rule fired: Windows account enabled, level medium (Event ID 4720)
Rule fired: Windows group membership change, level medium (Event ID 4728), IT Admin added to Domain Admins
Mimikatz: Not detected by SIEM but discovered during manual investigation of DC01 following account creation alert

## Investigation
* Wazuh alerted on Event ID 4722, a new user named IT Admin was enabled outside of normal business hours. A second alert followed for Event ID 4728 indicating IT Admin was added to the Domain Admins security group. Severity was high, privileged account creation is a strong indicator of compromise.
* Logged into DC01 to investigate. Reviewed Active Directory Users and Computers, confirmed IT Admin account existed and was a member of Domain Admins. Account had no legitimate purpose.
* While investigating the filesystem, C:\Temp was found on disk, an unusual directory with no legitimate purpose on a domain controller. Inside, mimikatz.exe was found. PowerShell history confirmed the file was downloaded from 192.168.10.29 via Invoke-WebRequest and executed. Given that Mimikatz was executed with domain admin privileges on the DC, a DCSync attack was assumed and full credential compromise of the domain was declared.

## Impact
* Domain Administrator credentials compromised
* Privileged backdoor account (IT Admin) enabled in Domain Admins
* Mimikatz DCSync attack executed, all domain hashes exposed including:
* Administrator hash
* krbtgt hash, enables Golden Ticket attacks, granting persistent domain access even after password resets
* All user account hashes
* Full domain compromise assumed

## Response and Containment
* Disabled and removed IT Admin backdoor account from Active Directory
* Deleted mimikatz.exe and removed C:\Temp directory from DC01
* Reset all passwords
* Reset krbtgt account password twice — required to invalidate any Golden Tickets that may have been forged
* Audited Domain Admins group for any other unauthorized members
* Reviewed all recent logon events on DC01

## Recommendations
* Restrict RDP access to DC to trusted hosts only via firewall rule
* Enable Wazuh active response to block IPs after repeated RDP authentication failures
* Enforce least privilege, Administrator account used for RDP should not be necessary for day to day administration
