## Incident Report: Directory Traversal Attack Against DMZ Nginx Web Server
* Severity: Low
* Status: Detected, Analyzed, No Impact

## Incident Summary
* On June 22, 2026, a directory traversal attack was attempted from Kali Linux at 192.168.10.29 against the nginx web server hosted in the DMZ (192.168.2.10). The attack was detected by Snort IDS and forwarded to Wazuh SIEM. The attack was unsuccessful, nginx returned a 404 and no files were shown. No further activity was detected from the source IP.

## Date and Time
* Attack occured: June 23, 2026 at 12:55 AM
* Detected by Snort: June 23, 2026 at 12:55 AM
* Reviewed: June 23, 2026 at 1:02 AM

## Attack Details
* Attack type: Directory Traversal
* Tool used: curl
* Command: 'curl --path-as-is http://192.168.2.10/../../../etc/passwd'
* Source IP: 192.168.10.29 (Kali Linux, LAN)
* Destination IP: 192.168.2.10 (nginx, DMZ)
* Target: nginx web server, attempting to access /etc/passwd outside of web root
* MITRE ATT&CK: T1083 — File and Directory Discovery

## Detection Details
* Tool: Snort IDS on pfSense OPT1 (DMZ interface)
* Rule fired: SID 119:18 — WEBROOT DIRECTORY TRAVERSAL
* Priority: 3
* Wazuh: Snort alert forwarded via syslog

# Investigation
