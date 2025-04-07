
# 💥 CAP - HackTheBox Writeup

> ⚠️ **Disclaimer**
>
> This report documents a security assessment performed on a publicly available vulnerable machine within the HackTheBox platform.
> All activities were conducted legally and within the authorized scope of HackTheBox Labs.
>
> This content is intended strictly for educational and ethical purposes only.

## 📌 Target Info
- **IP:** 10.10.10.245
- **Platform:** HackTheBox
- **Difficulty:** Easy
- **Method:** VPN-based access to HTB network

---

## 🧭 Enumeration

### 🔍 Nmap Scan

**Command:**
```bash
nmap -sC -sV -p- -oN cap_nmap.txt 10.10.10.245
```

**Key Findings:**
- `21/tcp` — FTP (vsftpd 3.0.3)
- `22/tcp` — SSH (OpenSSH 8.2p1 Ubuntu)
- `80/tcp` — HTTP (Gunicorn) with “Security Dashboard” title

### 🌐 Web Discovery

**Manual Inspection & Gobuster:**
- Found `/data/0`, `/data/3`
- `/data/3` contained a downloadable `.pcap` file

---

## 💣 Exploitation

### ✅ 1. PCAP Analysis → FTP Creds

**Tool:**
```bash
wireshark login.pcap
```

**Extracted Credentials:**
```
USER nathan
PASS Buck3tH4TF0RM3!
```

**Usage:**
```bash
ssh nathan@10.10.10.245
```

**Result:** SSH access as `nathan`.

**Flag #1:**
```bash
cat ~/user.txt
```

---

### ✅ 2. PrivEsc via pkexec (PwnKit)

**Command:**
```bash
pkexec --version
→ 0.105
```

**Vulnerability:** CVE-2021-4034 (PwnKit)

**Exploit Used:**
- GitHub PoC: https://github.com/berdav/CVE-2021-4034
- Compiled with `make`
- Uploaded and executed on target

**Result:**
```bash
./pwnkit
whoami → root
```

**Flag #2:**
```bash
cat /root/root.txt
```

---

## 🔐 Credentials Used

- SSH: `nathan : Buck3tH4TF0RM3!`

---

## 🧠 Lessons Learned

- PCAP files can leak credentials — always analyze available data
- Common misconfigs (e.g. pkexec version) are still present in live targets
- Privesc requires consistent recon of SUID binaries and versions

---

## 🧰 Tools Used

- `nmap`
- `gobuster`
- `wireshark`
- `ssh`
- `git`, `make`, `gcc`

---

## ✅ Outcome

The target machine was fully compromised via:
1. Extracted FTP creds from `.pcap` file → SSH access
2. Privilege escalation using the PwnKit (CVE-2021-4034) vulnerability

---

**Author:** Esteban BREA HELL  
**GitHub:** [github.com/EstebanBreaHell](https://github.com/EstebanBreaHell)
