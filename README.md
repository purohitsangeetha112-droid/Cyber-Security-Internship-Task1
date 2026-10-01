# Cyber Security Internship — Task 1
## Local Network Port Scanning & Reconnaissance

**Author:** Sangu  
**Date:** October 1, 2026  
**Platform:** Kali Linux (VirtualBox — Bridged Adapter)  
**Tool:** Nmap 7.99  

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Objective](#2-objective)
3. [Environment & Setup](#3-environment--setup)
4. [Methodology — What I Did](#4-methodology--what-i-did)
5. [Scan Results](#5-scan-results)
6. [Risk Analysis](#6-risk-analysis)
7. [What I Learned](#7-what-i-learned)
8. [Tools & Technologies Used](#8-tools--technologies-used)
09. [Conclusion](#09-conclusion)
10. [evidence](#10-evidence)

---

## 1. Executive Summary

This report documents a network reconnaissance exercise performed on a home network (192.168.1.0/24) using Nmap. The scan identified 7 live hosts, including a router, smart TV, streaming device, IoT hub, and a mobile device. A high-risk finding was discovered: an IoT device exposing a PostgreSQL database (port 5432) on the local network. This report details the methodology, findings, risk assessment, and key lessons learned.

---

## 2. Objective

To learn how to discover open ports on devices within a local network and understand network exposure using Nmap — a fundamental skill in cybersecurity reconnaissance.

---

## 3. Environment & Setup

| Component | Details |
|-----------|---------|
| Attacker Machine | Kali Linux (VirtualBox VM) |
| Network Mode | Bridged Adapter (Realtek Wi-Fi 6 PCIe NIC) |
| Kali IP Address | 192.168.1.2 |
| Target Range | 192.168.1.0/24 |
| Tool Version | Nmap 7.99 |
| Host OS | Windows 11 |

Why Bridged Mode?
Bridged mode connects the virtual machine directly to the physical network, allowing it to obtain a real IP from the home router and see all other devices — essential for realistic reconnaissance.

---

## 4. Methodology — What I Did

1. Installed Nmap on Kali Linux and verified the installation with nmap --version.
2. Identified the local IP range using ip addr — discovered 192.168.1.2/24.
3. Switched VirtualBox to Bridged Adapter to gain access to the real home network.
4. Executed a TCP SYN scan across the entire subnet:

    sudo nmap -sS -T4 --max-retries 1 --host-timeout 30s 192.168.1.0/24 -oN scan_results.txt

5. Performed service version detection on key hosts:

    sudo nmap -sS -sV -p 135,445,5432 <target> -oN scan_results_full.txt

6. Analyzed findings and identified high-risk open ports.
7. Documented results and saved outputs to text files for submission.

---

## 5. Scan Results

7 live hosts discovered on the network:

| IP Address | MAC Vendor | Likely Device | Open Ports | Risk Level |
|------------|-----------|---------------|-----------|-----------|
| 192.168.1.1 | Taicang T&W Electronics | Router / DSL Gateway | 80 (HTTP), 443 (HTTPS) | Medium |
| 192.168.1.5 | Samsung Electronics | Smart TV | 7000, 8001, 8002, 8080, 9080, 9110 | Medium |
| 192.168.1.8 | Unknown | Firewalled Device | (all filtered) | Low |
| 192.168.1.9 | Earda Technologies | Streaming / Smart Device | 8008, 8009 (AJP13), 8443, 9000 | Medium-High |
| 192.168.1.10 | AzureWave Technology | IoT Device (Camera/Hub) | 5432 (PostgreSQL) | HIGH |
| 192.168.1.11 | Random MAC | Phone / Laptop | (all closed) | Low |
| 192.168.1.2 | (self) | Kali VM | Skipped | N/A |

---

## 6. Risk Analysis

HIGH RISK — 192.168.1.10 (PostgreSQL on port 5432)
An IoT device is exposing a PostgreSQL database on the local network. This is highly unusual and dangerous:
- Default credentials (postgres:postgres) may allow unauthorized access.
- Unpatched PostgreSQL versions contain known CVEs.
- Recommendation: Identify the device, disable the database service if unused, and restrict access via firewall rules.

MEDIUM-HIGH RISK — 192.168.1.9 (AJP13 on port 8009)
Port 8009 is the Apache JServ Protocol — vulnerable to Ghostcat (CVE-2020-1938), allowing file read and potential RCE.
- Recommendation: Block port 8009 at the router, update firmware.

MEDIUM RISK — 192.168.1.1 (Router admin panel)
The router exposes HTTP (80) and HTTPS (443) admin interfaces.
- Recommendation: Use a strong admin password, disable remote management, keep firmware updated.

LOW RISK — 192.168.1.11
All scanned ports closed — indicates a device with a strong firewall posture (likely a MAC-randomized phone).

---

## 7. What I Learned

Through this task, I gained practical experience in:

- Network Reconnaissance: Understanding how to map live hosts and services on a subnet.
- Nmap Mastery: Using -sS (SYN scan), -sV (version detection), -O (OS fingerprinting), -T4 (timing), and -oN (output to file).
- TCP/IP Fundamentals: How the TCP three-way handshake works and why a half-open SYN scan is stealthier.
- Port & Service Awareness: Recognizing common ports (80, 443, 445, 5432) and the services behind them.
- Risk Assessment: Translating raw scan data into meaningful security findings.
- Virtualization Networking: The difference between NAT and Bridged mode, and why Bridged is required for real network scans.
- IoT Security Risks: Realizing that home IoT devices often expose unnecessary services — a real-world attack surface.
- Documentation Skills: Writing professional security reports for stakeholders.

---

-

## 8. Tools & Technologies Used

- Nmap 7.99 — Port scanning and service detection
- Kali Linux — Reconnaissance platform
- Oracle VirtualBox — Virtualization (Bridged Adapter)
- Git & GitHub — Version control and documentation

---

## 9. Conclusion

This task provided hands-on experience with network reconnaissance — a core skill in cybersecurity. By scanning a real home network, I discovered multiple devices with exposed services, including a high-risk PostgreSQL database on an IoT device. The exercise reinforced the importance of regular network audits, firewall configuration, and IoT device hardening.

Key Takeaway:
A home network is not as safe as it seems. Every open port is a potential doorway for an attacker — visibility is the first step to defense.

---

End of Report


## 10.EVIDENCE
<img width="1920" height="922" alt="IP ADRESS" src="https://github.com/user-attachments/assets/9dd12082-bdee-4070-a163-dd5ee614f802" />
<img width="1920" height="922" alt="SCAN HOME NETWORK" src="https://github.com/user-attachments/assets/7e06cba0-e41b-4ece-a4fa-fc7fa4538eb8" />
<img width="1920" height="922" alt="REULT PROJECT" src="https://github.com/user-attachments/assets/217d7a79-3078-4e23-a03d-82439ec5b084" />
