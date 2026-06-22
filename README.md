# Cybersecurity Home Lab

A personal cybersecurity and infrastructure home lab built in VMware Workstation on a custom-built desktop. Built to practice networking, system administration, security monitoring, Active Directory administration, firewalling, segmentation, and cybersecurity skills through hands-on work. Includes notes and screenshots from each lab session documenting troubleshooting steps and findings.

## Environment

**Host:** Custom-built desktop
**CPU:** Ryzen 7 7800X3D
**RAM:** 32GB
**Hypervisor:** VMware Workstation

## Network Architecture

```text
WAN / NAT
   |
pfSense Firewall
   |
   |-- LAN: 192.168.10.0/24
   |     |-- Kali Linux
   |     |-- Wazuh Manager
   |     |-- Windows Server / Active Directory Domain Controller
   |
   |-- DMZ: 192.168.2.0/24
         |-- Ubuntu nginx Web Server
```

## What I've Done So Far

* Built a virtualized LAN and DMZ network using pfSense
* Configured pfSense firewall rules to control traffic between LAN and DMZ
* Deployed an Ubuntu nginx web server in the DMZ
* Configured UFW firewall rules and tested exposed services with nmap
* Set up SSH and tested authentication logging
* Simulated basic incident response by terminating an active SSH session
* Diagnosed and fixed networking issues involving routing, DHCP, firewall rules, and Linux/Windows system configuration
* Deployed Wazuh SIEM on an Ubuntu server
* Installed Wazuh agents on lab machines and verified agent connectivity
* Built a Windows Server Active Directory domain controller
* Reviewed exposed services and made security decisions to reduce unnecessary attack surface
* Installed and configured Snort on pfSense to monitor DMZ traffic
* Validated Snort detection by generating test traffic from Kali to the DMZ web server and confirming IDS alerts in pfSense
* Forwarded pfSense firewall and Snort IDS logs to Wazuh SIEM for centralized visibility
* Wrote custom Wazuh correlation rule detecting SSH brute force followed by successful login (MITRE T1110, T1078)
* Simulated full attack chain: directory traversal, SSH brute force, successful login, and suspicious file creation
* Detected and triaged Shellshock false positive alert, confirmed source as authorized internal scan
* Simulated SYN flood DoS attack and confirmed nginx resilience

## Coming Soon

* More attack simulations and incident reports
* Honeypot deployment
* LAN-side IDS coverage
* DVWA vulnerable web application for SQL injection and XSS simulation
* AWS cloud security lab

## Certifications

* CompTIA Network+ (N10-009)
* CompTIA Security+ (SY0-701)
