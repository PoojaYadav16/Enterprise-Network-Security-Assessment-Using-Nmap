# Enterprise Network Security Assessment Using Nmap

## 1. Project Overview

This project demonstrates a network security assessment using Nmap in an isolated virtual lab.

The objective was to identify active devices, discover open TCP and UDP ports, identify running services and software versions, detect the target operating system, investigate security risks, and apply a firewall restriction.

## 2. Lab Environment

| Component       | Details          |
| --------------- | ---------------- |
| Scanner machine | Kali Linux       |
| Target machine  | Metasploitable 2 |
| Scanner IP      | 192.168.202.129  |
| Target IP       | 192.168.202.128  |
| Network         | 192.168.202.0/24 |
| Scanning tool   | Nmap 7.95        |
| Virtual network | VMware Host-only |

All testing was performed on an authorized, isolated lab network.

## 3. Assessment Methodology

### Step 1: Host Discovery

Command: `sudo nmap -sn 192.168.202.0/24`

Identified four active devices on the lab network, including the Kali scanner and Metasploitable target.

### Step 2: TCP Port Scanning

Command: `sudo nmap -sS 192.168.202.128`

Identified 23 open TCP ports in Nmap's default 1,000-port scan.

Important exposed services included FTP, SSH, Telnet, HTTP, SMB, MySQL, PostgreSQL, VNC and other legacy services.

### Step 3: Service and Version Detection

Command: `sudo nmap -sV 192.168.202.128`

Identified software versions, including:

- vsftpd 2.3.4
- OpenSSH 4.7p1
- Apache 2.2.8
- MySQL 5.0.51a
- PostgreSQL 8.3
- ProFTPD 1.3.1

Nmap also identified port 1524 as a Metasploitable root shell.

### Step 4: Operating System Detection

Command: `sudo nmap -O 192.168.202.128`

Nmap estimated the target operating system as Linux kernel 2.6.9–2.6.33.

### Step 5: UDP Port Scanning

Command: `sudo nmap -sU --top-ports 20 192.168.202.128`

Identified open UDP ports:

- 53 — DNS
- 137 — NetBIOS name service

Additional ports 68, 69 and 138 were reported as open|filtered and require further investigation.

### Step 6: Risk Identification

Important security findings:

1. Telnet on port 23: Insecure remote-login protocol that does not encrypt communication.
2. Port 1524: Exposed root shell, representing a critical security concern.
3. FTP on port 21: Identified vsftpd 2.3.4, a version requiring urgent vulnerability review.
4. Database ports 3306 and 5432: Network-accessible database services requiring access-control review.
5. SMB on port 445: File-sharing service requiring security configuration review.

These findings are potential risks. Further validation is required before confirming exploitability.

### Step 7: Remediation — Telnet Firewall Restriction

On the Metasploitable target, applied the following firewall rule:

`sudo iptables -I INPUT 1 -p tcp --dport 23 -j DROP`

This rule drops incoming TCP traffic destined for port 23.

### Step 8: Remediation Verification

From Kali, executed:

`sudo nmap -sS -p 23 --reason 192.168.202.128`

Before remediation:

`23/tcp open telnet`

After remediation:

`23/tcp filtered telnet`

The result confirmed that traffic to Telnet port 23 was being filtered by the firewall.

Note: This firewall restriction did not uninstall or disable the Telnet service.

## 4. Recommendations

- Replace Telnet with secure remote administration using SSH.
- Investigate and remove unnecessary exposed services.
- Review and patch outdated software.
- Restrict database access to authorized systems.
- Review SMB and NetBIOS access.
- Investigate the exposed root shell on port 1524 immediately.
- Maintain firewall rules and regularly perform security assessments.

## 5. Conclusion

This project provided practical experience in network discovery, TCP and UDP scanning, service identification, operating system detection, risk identification, firewall remediation and verification.

Using Nmap, I identified multiple exposed services on the target server, investigated potential security risks, applied a firewall restriction to Telnet and verified the change through a second scan.

The project demonstrates a basic security assessment workflow: Discover, Scan, Investigate, Remediate and Verify.
