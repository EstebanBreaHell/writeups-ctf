
# 🧠 OverTheWire - Bandit Walkthrough (Levels 0 to 14)

This walkthrough covers Bandit levels 0 to 14 with the commands and passwords used to complete each level.

---

### 🔐 Level 0
```bash
cat readme
# Password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

---

### 🔐 Level 1
```bash
cat < -
# Password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

---

### 🔐 Level 2
```bash
cat "spaces in this filename"
# Password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

---

### 🔐 Level 3
```bash
ls -a
cat .hidden/.hidden/.hidden/...Hiding-From-You
# Password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

---

### 🔐 Level 4
```bash
cat < -file07
# Password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

---

### 🔐 Level 5
```bash
find . -type f -size 1033c ! -executable
file ./maybehere07/.file2
cat ./maybehere07/.file2
# Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

---

### 🔐 Level 6
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
# Password: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

---

### 🔐 Level 7
```bash
cat data.txt | grep "millionth"
# Password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

---

### 🔐 Level 8
```bash
sort data.txt | uniq -u
# Password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

---

### 🔐 Level 9
```bash
strings data.txt
# Password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

---

### 🔐 Level 10
```bash
base64 -d data.txt
# Password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

---

### 🔐 Level 11
```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
# Password: 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

---

### 🔐 Level 12
```bash
# Loop through ZIP extractions
cat data
# Password: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

---

### 🔐 Level 13
```bash
ssh -i sshkey.private -p 2220 bandit14@localhost
```

---

### 🔐 Level 14
```bash
cat /etc/bandit_pass/$(whoami)
echo MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS | nc localhost 30000
# Password: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

---

Feel free to improve or clean it further for GitHub — but this is ready to drop into your `writeups-ctf` repo!
