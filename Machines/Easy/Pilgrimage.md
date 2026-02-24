# HTB — Pilgrimage

**Official HTB page**: [Pilgrimage](https://app.hackthebox.com/machines/Pilgrimage?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 24 Jun, 2023  
**Platforms**: HackTheBox  
**Tags**: Other

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – nginx 1.18.0)  
- Website redirected to `pilgrimage.htb`  
- Directory enumeration revealed exposed `.git` repository  

---

## Vulnerability Identified
- Extracted source code using `git-dumper`  
- Discovered a local `magick` binary (ImageMagick 7.1.0-49 beta)  
- Version vulnerable to **CVE-2022-44268** (Arbitrary File Read via crafted PNG)  

---

## Initial Access
- Crafted malicious PNG embedding `/etc/passwd` using `pngcrush`  
- Uploaded image to the application  
- Downloaded processed image and extracted embedded file content  
- Repeated attack to read application database file  
- Reconstructed SQLite database from extracted hex data  
- Retrieved credentials for user `emily`  
- Gained SSH access  

---

## Privilege Escalation
- Observed root executing `/usr/sbin/malwarescan.sh`  
- Script used vulnerable **Binwalk v2.3.2**  
- Exploited **CVE-2022-4510 (WalkingPath)**  
- Crafted malicious image to trigger command execution  
- Achieved root access  

---

## Key Takeaways
- Exposed `.git` repositories can reveal sensitive application internals  
- ImageMagick vulnerabilities can lead to arbitrary file read  
- Reconstructed SQLite databases may contain reusable credentials  
- Automated root scripts using vulnerable tools can provide privilege escalation paths  
