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
* Command: curl --path-as-is http://192.168.2.10/../../../etc/passwd
* Source IP: 192.168.10.29 (Kali Linux, LAN)
* Destination IP: 192.168.2.10 (nginx, DMZ)
* Target: nginx web server, attempting to access /etc/passwd outside of web root
* MITRE ATT&CK: T1083 — File and Directory Discovery

## Detection Details
* Tool: Snort IDS on pfSense OPT1 (DMZ interface)
* Rule fired: SID 119:18 — WEBROOT DIRECTORY TRAVERSAL
* Priority: 3
* Wazuh: Snort alert forwarded via syslog

## Investigation
* Reviewed Snort alert, source IP confirmed as Kali on LAN. Reviewed nginx access log DMZ server, confirmed server returned 404 for the request. The --path-as-is flag was used to prevent curl from normalizing the path before sending, confirming that the raw ../ sequences reached the server. nginx correctly restricted access outside the web root and did not reveal any files. No follow-up requests were observed from the same source IP.

## Impact
* None. Attack was unsuccessful. nginx config correctly prevented directory traversal. No files were disclosed.

## Response and Containment
* No containment required, attack did not succeed. In a real environment: block source IP at the firewall, review web server configuration for path traversal misconfigurations, check for other requests from the same IP indicating broader reconnaissance.

## Recommendations
* Enable Snort's IPS to block request
* Periodically audit nginx configuration to ensure the root directive is properly scoped
* Create a Wazuh correlation rule to escalate repeated traversal attempts from the same source IP as potential active reconnaissance
