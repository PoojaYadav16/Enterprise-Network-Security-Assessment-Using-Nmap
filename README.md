<p align="center">
  <img src="nmap project banner.png" alt="Enterprise Network Security Assessment Using Nmap" width="100%">
</p>

<h1 align="center">Enterprise Network Security Assessment Using Nmap</h1>

<p align="center">
  <strong>Discover • Analyze • Secure</strong>
</p>

---

## 📌 Project Overview

This project demonstrates a practical network security assessment using Nmap in an isolated VMware lab environment.

The objective was to identify active hosts, discover open ports, detect running services, estimate the operating system, assess UDP ports, identify potential security risks, and apply and verify a firewall-based remediation.

## 🛠️ Tools and Technologies

- Kali Linux
- Nmap 7.95
- Metasploitable 2
- VMware Workstation
- Linux iptables Firewall

## 🖥️ Lab Environment

| Component | Configuration |
|---|---|
| Scanner Machine | Kali Linux |
| Target Machine | Metasploitable 2 |
| Scanner IP | 192.168.202.129 |
| Target IP | 192.168.202.128 |
| Network | 192.168.202.0/24 |
| Network Type | VMware Host-only |

## 🔍 Project Workflow

### 1. Asset Discovery

- Identified active devices on the lab network.
- Used Nmap host discovery.
- Discovered the target machine at 192.168.202.128.

### 2. TCP Port Assessment

- Performed TCP SYN scanning.
- Identified 23 open TCP ports in the default top 1,000 TCP port scan.
- Identified services that required further investigation.

### 3. Service and Version Detection

- Detected running services and their reported versions.
- Identified FTP, SSH, Telnet, HTTP, SMB, MySQL, PostgreSQL and other services.
- Reviewed potentially outdated services.

### 4. Operating System Detection

- Used Nmap OS detection.
- Received an estimated Linux kernel fingerprint.
- Recorded the OS detection results for further review.

### 5. UDP Port Assessment

- Scanned the top 20 UDP ports.
- Identified UDP 53 (DNS) and UDP 137 (NetBIOS Name Service) as open.
- Identified UDP 68, 69 and 138 as open|filtered.

### 6. Security Remediation

Identified Telnet on TCP port 23 as an insecure remote-login service because it does not encrypt communication.

Applied this firewall rule on the authorized target machine:

```bash
sudo iptables -I INPUT 1 -p tcp --dport 23 -j DROP
