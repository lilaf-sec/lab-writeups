# HTB — Networked

**Official HTB page**: [Networked](https://app.hackthebox.com/machines/Networked?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 24 Aug, 2019  
**Platforms**: HackTheBox  
**Tags**: File Upload

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – Apache 2.4.6, PHP 5.4.16)  
- Directory enumeration revealed:  
  - `upload.php`  
  - `photos.php`  
  - `uploads/`  
  - `lib.php`  

- Source code review exposed the upload validation logic  

---

## Vulnerability Identified
- The file upload mechanism performed weak extension validation  
- Bypass achieved using a double extension technique  
- Allowed upload of a PHP webshell disguised as an image  

---

## Initial Access
- Uploaded a crafted file containing embedded PHP code  
- Executed commands via webshell  
- Obtained reverse shell as the web server user  

---

## Lateral Movement
- Discovered a cron job executed by a higher-privileged user  
- File name injection allowed command execution  
- Abused the scheduled task to obtain access as the `guly` user  

---

## Privilege Escalation
- `sudo -l` revealed permission to execute `/usr/local/sbin/changename.sh` as root  
- Script improperly sanitized user input  
- Injected command through interface name field  
- Spawned a root shell  

---

## Key Takeaways
- File upload filters are often bypassable with extension tricks  
- Embedded PHP inside image files remains a common attack vector  
- Cron jobs must validate filenames and user-controlled input  
- Sudo scripts without strict input handling lead to full compromise  
