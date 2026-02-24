# HTB — Previse

**Official HTB page**: [Previse](https://app.hackthebox.com/machines/Previse?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 07 Aug, 2021  
**Platforms**: HackTheBox  
**Tags**: Other

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – Apache 2.4.29)  
- Web application redirected to `login.php`  
- `PHPSESSID` cookie missing `httponly` flag  
- Directory enumeration revealed multiple PHP endpoints  

---

## Vulnerability Identified
- Application allowed account registration without proper access control  
- A downloadable site backup exposed the full source code  
- Source code revealed:
  - Hardcoded MySQL credentials  
  - An RCE vulnerability in `file_logs.php`  

---

## Initial Access
- Created a user account and authenticated  
- Downloaded the exposed site backup  
- Identified RCE vector within application code  
- Connected to MySQL using discovered credentials  
- Extracted password hash from database  
- Cracked hash to obtain valid system credentials  
- Gained SSH access  

---

## Privilege Escalation
- `sudo -l` revealed a root script used for log backups  
- Script executed `gzip` without absolute path  
- Abused **PATH hijacking** by creating a malicious `gzip` binary  
- Executed backup script with modified PATH  
- Achieved root access  

---

## Key Takeaways
- Source code disclosure often leads to full compromise  
- Backup files may expose credentials and hidden vulnerabilities  
- Database credential reuse can provide SSH access  
- Sudo scripts without absolute paths are vulnerable to PATH hijacking  
