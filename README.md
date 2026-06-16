# Cisometric Academy CTF - Penetration Testing Write-Up

## 🎯 Overview
This repository contains my comprehensive technical documentation and exploitation walkthrough for the **Cisometric Academy CTF**[cite: 1]. The challenge consisted of 8 distinct target machines (Linux and Windows) designed to simulate a corporate network environment[cite: 1]. 

I successfully compromised all 8 machines[cite: 1], executing full kill-chain operations from initial reconnaissance to root/system privilege escalation[cite: 1].

📄 **[Read the Full 30+ Page Technical PDF Here](./1301220371-Cisometric_Academy-ctf.pdf)**

---

## 🛠️ Key Skills & Methodologies Demonstrated
* **Network Enumeration:** Nmap, SMBmap, RPC/NFS analysis, WPScan, Gobuster[cite: 1].
* **Web Application Exploitation:** PHP 8.1.0-dev Backdoor exploitation, exposed `.git` directory dumping, Authenticated File Upload bypass (Magic Bytes manipulation)[cite: 1].
* **Authentication & Cryptography:** 2FA/OTP bypass using exposed `google_authenticator.bak` files, Vigenère cipher decoding, NTLM hash analysis[cite: 1].
* **Privilege Escalation:** 
  * Linux Kernel Exploits (CVE-2022-0847 Dirty Pipe, Linux Kernel 2.4.x `ptrace/kmod`)[cite: 1].
  * Linux Capabilities Abuse (`cap_dac_read_search` with Python, `cap_setuid` with Vim)[cite: 1].
  * SUID Binary Manipulation (`sed`, `php8.3`)[cite: 1].

---

## 🖥️ Target Machines Breakdown

### 1. Capitano (Linux)
* **Initial Access:** Anonymous FTP login leading to a data leak (PCAP file)[cite: 1]. Extracted valid credentials by following TCP streams in Wireshark[cite: 1].
* **Privilege Escalation:** Exploited Linux Capabilities (`cap_dac_read_search`) using Python to read restricted root directories and files[cite: 1].

### 2. Distintivo (Linux)
* **Initial Access:** Mounted a public NFS share to discover a leaked 2FA backup file[cite: 1]. Spoofed UID to read the file and generated a time-based OTP via `oathtool` to bypass SSH 2FA[cite: 1].
* **Privilege Escalation:** Extracted SQLite database credentials[cite: 1]. Exploited an insecure `sudo` configuration allowing the execution of `sed` without a password to spawn a root shell[cite: 1].

### 3. Dothraki (Linux)
* **Initial Access:** Identified a vulnerable PHP 8.1.0-dev installation via `info.php`[cite: 1]. Leveraged the `User-Agentt` backdoor to achieve Remote Code Execution (RCE) and spawn a reverse shell[cite: 1].
* **Privilege Escalation:** Compiled and executed the CVE-2022-0847 (Dirty Pipe) kernel exploit via Metasploit to hijack the `/usr/bin/su` binary and achieve root[cite: 1].

### 4. Elyria (Linux)
* **Initial Access:** Dumped an exposed `.git` repository using `git-dumper` to uncover source code and hashed credentials[cite: 1]. Bypassed file upload restrictions by disguising a PHP web shell as a `.gif` image (magic bytes)[cite: 1].
* **Privilege Escalation:** Exploited the `cap_setuid` capability on `/usr/bin/vim.basic` using Python support to escalate to root[cite: 1].

### 5. Limited (Linux)
* **Methodology:** Conducted deep enumeration on a WordPress instance using WPScan and Gobuster, investigating XML-RPC, RSS feeds, and outdated themes (Twenty Twenty-Five)[cite: 1].

### 6. Sakaratul (Linux)
* **Initial Access:** Enumerated legacy Samba and Apache (`mod_ssl`) services[cite: 1]. Attempted `trans2open` and OpenFuck exploits[cite: 1].
* **Privilege Escalation:** Compiled and deployed a local privilege escalation exploit for the outdated Linux Kernel (2.4.x) utilizing `ptrace/kmod` to gain root access[cite: 1].

### 7. SimpleGuy (Linux)
* **Initial Access:** Utilized anonymous FTP to upload a customized PHP reverse shell directly into the web server's `/upload` directory[cite: 1].
* **Privilege Escalation:** Stabilized the Netcat shell using Python `pty`[cite: 1]. Leveraged a misconfigured SUID bit on the `php8.3` binary to execute a CLI command that read the `root.txt` flag[cite: 1].

### 8. Unnamed Windows Host
* **Initial Access:** Enumerated SMB shares using `smbclient` and mapped SIDs using Impacket (`lookupsid.py`) to discover valid users[cite: 1].
* **Post-Exploitation Analysis:** Discovered a confidential military memo[cite: 1]. Decoded a Vigenère cipher using CyberChef to reveal a French operations report containing intercepted NTLM hashes for the Administrator and local users[cite: 1].

---

*This repository is for educational and professional portfolio purposes. All exploitation was conducted within a legally authorized CTF environment.*
