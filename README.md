# Homelab-Blue-Red-Team-Training-
# Homelab (Blue & Red Team Training)

This repository documents a hands-on cybersecurity homelab environment built to support both offensive (Red Team) and defensive (Blue Team) skill development. Using Kali Linux, Metasploitable 2, Windows 10 (hardened), and Windows 7 virtual machines in a local VMware environment, this lab simulates real-world attack scenarios, detection workflows, and system hardening tasks.

The lab is designed to support career preparation for:
- Security Operations Center (SOC) Analyst roles
- Cybersecurity Analyst positions
- Entry-level Cloud Security roles

Cloud-based scenarios and AWS security modules are planned for future phases of this project.

## Lab Architecture

- All virtual machines are run locally using VMware Workstation
- Host-only network is used to isolate and simulate internal threat activity
- VMs include: Kali, Metasploitable 2, Windows 10, and Windows 7

## Lab Categories

| Category             | Description                                                       |
|----------------------|-------------------------------------------------------------------|
| Reconnaissance       | Nmap scans, OS fingerprinting, and enumeration of services        |
| Exploitation         | Remote exploits using Metasploit and manual payload execution     |
| Web App Attacks      | DVWA + Burp Suite for XSS, SQLi, and input manipulation           |
| Brute Force          | Hydra for SSH and HTTP credential testing                         |
| Post-Exploitation    | Enumeration and privilege escalation in Windows and Linux targets |
| Detection & Logging  | Analysis using Event Viewer, Sysmon, and simulated alerting       |
| Hardening            | Secure configuration of Windows 10 and Linux baselines            |

## Tools Used

- Kali Linux: Nmap, Hydra, Burp Suite, Metasploit, Nikto
- Windows 10/7: PowerShell, Event Viewer, Sysmon
- Linux (Ubuntu/Metasploitable): Bash, UFW, Fail2Ban, Netcat
- Splunk, Snort, pfSense 

