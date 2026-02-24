# HTB — Tenten

**Official HTB page**: [Tenten](https://app.hackthebox.com/machines/Tenten?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 22 Mar, 2017  
**Platforms**: HackTheBox  
**Tags**: CMS, Idor, steganography, python  

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH – OpenSSH 7.2p2 Ubuntu)
  - 80 (HTTP – Apache 2.4.18)
- Web server running WordPress 4.7.3
- Identified valid user `takis` via WordPress login response
- Job portal functionality exposed:
  - `/index.php/jobs/pen-tester/`
  - `/index.php/jobs/apply/8/`
- IDOR discovered on `/jobs/apply/<id>/`
- Enumerated multiple IDs and found:
  - `HackerAccessGranted`

---

## Vulnerability Identified
- Directory fuzzing revealed `job-manager` plugin
- Identified vulnerability **CVE-2015-6668**
- Vulnerability allowed CV filename disclosure
- Recreated public exploit in Python3
- Enumerated upload directories by year and month
- Located uploaded file:
  - `/wp-content/uploads/2017/04/HackerAccessGranted.jpg`

---

## Initial Access
- Downloaded discovered image file
- Performed steganography analysis
- Extracted embedded `id_rsa` private key
- Cracked passphrase using ssh2john
- Logged in via SSH as `takis`

---

## Privilege Escalation
- `sudo -l` revealed:
  - `(ALL : ALL) ALL`
  - `(ALL) NOPASSWD: /bin/fuckin`
- Inspected `/bin/fuckin`
- Script executed positional arguments directly
- Ran:
  - `sudo /bin/fuckin bash`
- Spawned root shell

---

## Key Takeaways
- WordPress plugins are frequent attack vectors
- IDOR vulnerabilities can expose sensitive internal references
- CVE-2015-6668 allows predictable file disclosure
- Steganography can hide credentials in uploaded files
- Poorly written sudo scripts can grant instant root access
