# HTB — Blocky

**Official HTB page**: [Blocky](https://app.hackthebox.com/machines/Blocky)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 21 Jul, 2017  
**Platforms**: HackTheBox  
**Tags**: CMS

---

## Initial Enumeration
- Nmap revealed ports:
  - 21 (FTP)
  - 22 (SSH)
  - 80 (HTTP)
  - 25565 (Minecraft server)
- The web server redirected to `blocky.htb`
- WordPress was identified
- A user named `notch` was visible on the website

---

## Web Enumeration
- Directory fuzzing revealed:
  - `/wiki`
  - `/wp-content`
  - `/wp-includes`
  - `/plugins`
- The `/plugins` directory contained Java `.jar` files

---

## Vulnerability Identified — Hardcoded Credentials
- The `.jar` files were decompiled
- Source code revealed hardcoded database credentials
- The exposed password was reused by system user `notch`
- SSH access was obtained using the recovered credentials

---

## Privilege Escalation
- User `notch` had sudo privileges
- The recovered password allowed sudo authentication
- Root access was obtained directly via sudo

---

## Key Takeaways
- Exposed plugin files can leak sensitive information
- Hardcoded credentials remain a common security failure
- Credential reuse between services leads to full compromise
- Simple misconfigurations can make exploitation trivial
