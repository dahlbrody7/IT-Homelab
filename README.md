# Home Lab

A personal IT homelab built using VMware Workstation on a custom built PC. Built to get comfortable troubleshooting and to learn networking, system administration, security, and IT infrastructure through hands-on experience. Includes notes from each lab session that document troubleshooting along with screenshots.

## Environment
- Host: Custom built desktop (Ryzen 7 7800X3D, 32GB RAM)
- Hypervisor: VMware Workstation
  
## Architecture

```
LAN (192.168.10.x)
  Kali        Wazuh
     \        /
      \      /
       pfSense
          |
DMZ (192.168.2.x)
        Ubuntu
```

## What I've Done So Far
- Deployed and configured nginx web server
- Configured UFW firewall rules and tested with nmap
- Set up SSH and tested authentication logging
- Simulated basic incident response by terminating an active SSH session
- Diagnosed and resolved network issues on Ubuntu and Kali Linux VMs
- Built a virtualized network with pfSense, with LAN + DMZ segmentation
- Troubleshot connectivity problems stemming from routing, DHCP, firewall, and system issues
- Deployed Wazuh SIEM on a new Ubuntu VM and verified installation

## Coming Soon
Connecting Ubuntu server to Wazuh and testing that logs and alerts work properly
Implementing a Windows Server machine with Active Directory
IDS/IPS

## Certifications
- CompTIA Network+ (N10-009)
- CompTIA Security+ (SY0-701)
