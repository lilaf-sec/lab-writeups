# HTB — Bashed

**Official HTB page**: https://app.hackthebox.com/machines/Bashed?tab=play_machine

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 20 Dec, 2017  
**Platforms**: HackTheBox  
**Tags**: Python

---

## Initial Enumeration
- Nmap scan revealed open port:
  - 80 (HTTP – Apache)
- No other exposed services
- Web application hosted a blog page
- Blog post hinted at development files

---

## Vulnerability Identified

- Directory enumeration revealed:
  - `/dev`
- `/dev` contained a functional copy of **phpbash**
- phpbash provided command execution through web interface

---

## Initial Access

- Used phpbash to execute system commands
- Upgraded to full interactive shell
- Gained access as `www-data`

---

## Lateral Movement

- Enumeration revealed `/scripts` directory
- Directory owned by user `scriptmanager`
- `sudo -l` showed:
  - `www-data` could run commands as `scriptmanager` without password
- Switched to `scriptmanager`
- Gained full read/write access to `/scripts`

---

## Privilege Escalation

- Inside `/scripts`, observed Python files
- File timestamps indicated execution every minute
- `test.txt` owned by root
- Confirmed presence of root cron job executing scripts in `/scripts`
- Modified existing Python script
- Achieved root execution
- Gained root shell

---

## Key Takeaways
- Blog content can hint at hidden development paths
- Web shells like phpbash can lead directly to system compromise
- Misconfigured sudo rules enable lateral movement
- Writable directories executed by cron jobs are critical privilege escalation vectors
