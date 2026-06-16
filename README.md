# Cisometric Academy CTF - Penetration Testing Write-Up

## 🎯 Overview
This repository contains my comprehensive technical documentation and exploitation walkthrough for the **Cisometric Academy CTF**[cite: 1]. The challenge consisted of 8 distinct target machines (Linux and Windows) designed to simulate a corporate network environment. 

I successfully compromised all 8 machines, executing full kill-chain operations from initial reconnaissance to root/system privilege escalation.

📄 **[Read the Full 30+ Page Technical PDF Here](./1301220371-Cisometric_Academy-ctf.pdf)**

---

## 🛠️ Key Skills & Methodologies Demonstrated
* **Network Enumeration:** Nmap, SMBmap, RPC/NFS analysis, WPScan, Gobuster.
* **Web Application Exploitation:** PHP 8.1.0-dev Backdoor exploitation, exposed `.git` directory dumping, Authenticated File Upload bypass (Magic Bytes manipulation).
* **Authentication & Cryptography:** 2FA/OTP bypass using exposed `google_authenticator.bak` files, Vigenère cipher decoding, NTLM hash analysis.
* **Privilege Escalation:** 
  * Linux Kernel Exploits (CVE-2022-0847 Dirty Pipe, Linux Kernel 2.4.x `ptrace/kmod`).
  * Linux Capabilities Abuse (`cap_dac_read_search` with Python, `cap_setuid` with Vim).
  * SUID Binary Manipulation (`sed`, `php8.3`).

---

## 🖥️ Target Machines Breakdown

### 1. Capitano (Linux)
* **Initial Access:** Anonymous FTP login leading to a data leak (PCAP file). Extracted valid credentials by following TCP streams in Wireshark.
* **Privilege Escalation:** Exploited Linux Capabilities (`cap_dac_read_search`) using Python to read restricted root directories and files.

### 2. Distintivo (Linux)
* **Initial Access:** Mounted a public NFS share to discover a leaked 2FA backup file. Spoofed UID to read the file and generated a time-based OTP via `oathtool` to bypass SSH 2FA.
* **Privilege Escalation:** Extracted SQLite database credentials. Exploited an insecure `sudo` configuration allowing the execution of `sed` without a password to spawn a root shell.

### 3. Dothraki (Linux)
* **Initial Access:** Identified a vulnerable PHP 8.1.0-dev installation via `info.php`. Leveraged the `User-Agentt` backdoor to achieve Remote Code Execution (RCE) and spawn a reverse shell.
* **Privilege Escalation:** Compiled and executed the CVE-2022-0847 (Dirty Pipe) kernel exploit via Metasploit to hijack the `/usr/bin/su` binary and achieve root.

### 4. Elyria (Linux)
* **Initial Access:** Dumped an exposed `.git` repository using `git-dumper` to uncover source code and hashed credentials. Bypassed file upload restrictions by disguising a PHP web shell as a `.gif` image (magic bytes).
* **Privilege Escalation:** Exploited the `cap_setuid` capability on `/usr/bin/vim.basic` using Python support to escalate to root.

### 5. Limited (Linux)
* **Methodology:** Conducted deep enumeration on a WordPress instance using WPScan and Gobuster, investigating XML-RPC, RSS feeds, and outdated themes (Twenty Twenty-Five).

### 6. Sakaratul (Linux)
* **Initial Access:** Enumerated legacy Samba and Apache (`mod_ssl`) services. Attempted `trans2open` and OpenFuck exploits.
* **Privilege Escalation:** Compiled and deployed a local privilege escalation exploit for the outdated Linux Kernel (2.4.x) utilizing `ptrace/kmod` to gain root access.

### 7. SimpleGuy (Linux)
* **Initial Access:** Utilized anonymous FTP to upload a customized PHP reverse shell directly into the web server's `/upload` directory.
* **Privilege Escalation:** Stabilized the Netcat shell using Python `pty`. Leveraged a misconfigured SUID bit on the `php8.3` binary to execute a CLI command that read the `root.txt` flag.

### 8. Unnamed Windows Host
* **Initial Access:** Enumerated SMB shares using `smbclient` and mapped SIDs using Impacket (`lookupsid.py`) to discover valid users.
* **Post-Exploitation Analysis:** Discovered a confidential military memo. Decoded a Vigenère cipher using CyberChef to reveal a French operations report containing intercepted NTLM hashes for the Administrator and local users.

---

*This repository is for educational and professional portfolio purposes. All exploitation was conducted within a legally authorized CTF environment.*
