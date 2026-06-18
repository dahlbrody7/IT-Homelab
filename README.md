# Cybersecurity Home Lab

A personal cybersecurity and infrastructure home lab built in VMware Workstation on a custom-built desktop. Built to practice networking, system administration, security monitoring, Active Directory administration, firewalling, segmentation, and defensive security engineering through hands-on work.

The goal is to build a small enterprise-style environment where I can configure systems, break/fix issues, monitor activity, reduce attack surface, and document the security decisions made during each lab session.

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

## Security Skills Practiced

* Network segmentation
* Firewall rule design
* Service exposure review
* Attack surface reduction
* SIEM agent deployment
* IDS configuration and validation
* Log and alert review
* Basic Active Directory administration
* Troubleshooting security tooling when alerts or agents do not work as expected

## Tools and Technologies

* VMware Workstation
* pfSense
* Windows Server / Active Directory
* Ubuntu
* Kali Linux
* Wazuh
* Snort
* nginx
* UFW
* nmap
* PowerShell
* Bash

## Coming Soon

* Forward pfSense/Snort logs to Wazuh using syslog
* Tune Wazuh alerts to reduce noise and make important events easier to investigate
* Full IDS/IPS implementation
* Create and investigate security events
* Python Scripting/Automation for repetitive tasks, log parsing, and automation

## Certifications

* CompTIA Network+ (N10-009)
* CompTIA Security+ (SY0-701)
