# HTB — Instant

**Official HTB page**: [Instant](https://app.hackthebox.com/machines/Instant)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 12 Oct, 2024  
**Platforms**: HackTheBox  
**Tags**: API Abuse, LFI, Python

---

## Initial Enumeration
- Nmap revealed ports 22 (SSH) and 80 (HTTP)
- The web server redirected to `instant.htb`
- The website allowed downloading an Android application: `instant.apk`
- The APK was decompiled for analysis with `apktool`
- Hardcoded API endpoints and subdomains were discovered:
  - `mywalletv1.instant.htb`
  - `swagger-ui.instant.htb`

---

## API Enumeration
- The Swagger interface exposed administrative endpoints
- An endpoint allowed reading log files
- Access was restricted via API key authentication
- Further analysis of the APK source code revealed a hardcoded API key
- The recovered API key granted access to administrative API routes

---

## Vulnerability Identified — Arbitrary File Read
- The log reading endpoint was vulnerable to directory traversal
- Supplying `../../../../` sequences allowed reading arbitrary system files
- `/etc/passwd` confirmed file disclosure
- The private SSH key of user `shirohige` was retrieved from the home directory

---

## Initial Access
- The recovered private key allowed SSH access as `shirohige`
- A stable shell was obtained on the target system

---

## Privilege Escalation
- Enumeration revealed a Solar-PuTTY backup file:
  - `/opt/backups/Solar-PuTTY/sessions-backup.dat`
- The file contained encrypted session credentials
- The backup file was brute-forced offline
- Valid root credentials were recovered from the decrypted content
- SSH access as root was obtained

---

## Key Takeaways
- Mobile applications often expose sensitive backend information
- Hardcoded API keys in APK files are a critical security flaw
- Swagger interfaces frequently expose powerful administrative endpoints
- Directory traversal in file-reading APIs leads to full compromise
- Backup files can contain encrypted but recoverable credentials
- Credential reuse across services enables rapid privilege escalation
