## Incident Report: Successful SSH Brute-Force Against DMZ Nginx Web Server
* Severity: High
* Status: Detected, Analyzed, Contained and Resolved

## Incident Summary
* On June 23, 2026, an SSH brute-force attack was conducted from 192.168.10.29 against the DMZ nginx server at 192.168.2.10. The attack was detected by Wazuh SIEM via authentication log monitoring. The attacker successfully authenticated after exhausting a password list, then established an SSH session and created a suspicious file. The session was terminated and the file was removed.

## Date and Time
* Attack occured: June 23, 2026 at 6:05 AM
* Detected by Snort: June 23, 2026 at 6:05 AM
* Reviewed: June 23, 2026 at 6:12 AM

## Attack Details
* Attack type: SSH Brute Force
* Tool used: Hydra
* Source IP: 192.168.10.29 (Kali Linux, LAN)
* Destination IP: 192.168.2.10 (nginx, DMZ)
* Target: SSH service on port 22
* T1110.001, Brute Force: Password Guessing, T1078, Valid Accounts, T1105, Ingress Tool Transfer

## Detection Details
* Tool: Wazuh SIEM monitoring auth.log on DMZ agent
* Rule fired: 5712, SSH brute force detected, level 10 (medium)
* Custom rule fired: 100006, SSH brute force succeded, multiple failures followed by successful login from same IP, level 14 (High)
* Both alerts visible in Wazuh

## Investigation
* Reviewed Wazuh alerts,  rule 5712 fired indicating rapid successive authentication failures from 192.168.10.29 against user osboxes. The dashboard's medium severity count spiked from approximately 10 to 45 alerts during the attack. Custom correlation rule 100006 subsequently fired at level 14 when a successful authentication was recorded from the same source IP, confirming the brute force succeeded.
* Reviewed auth.log on the DMZ machine and confirmed the sequence of failed logins followed by a successful login from 192.168.10.29. Post-authentication, a file named invoice.pdf.exe was created in /Documents, a double extension masquerading technique commonly used to disguise executables as documents. Wazuh detected the file creation via real-time monitoring of /Documents.
No further lateral movement was detected. Source IP made no attempts to reach LAN-side hosts following the compromise.

## Impact
* Valid credentials obtained via brute force
* Interactive shell access established on DMZ server
* Suspicious file created in /Documents (invoice.pdf.exe)
* No data exfiltration detected
* No lateral movement to LAN

## Response and Containment
* Terminated active SSH session from source IP
* Removed invoice.pdf.exe from /Documents
* Verified no additional accounts were created on the DMZ machine
* Changed user passwords

## Recommendations
* Disable password-based SSH authentication, enforce key-based authentication only
* Implement fail2ban or Wazuh active response to automatically block IPs after repeated authentication failures
* Restrict SSH access to specific trusted source IPs via firewall rule
* Enable Snort IPS mode to block brute force traffic at the network level before it reaches the host

