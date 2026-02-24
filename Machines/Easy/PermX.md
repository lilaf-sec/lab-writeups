# HTB — PermX

**Official HTB page**: [PermX](https://app.hackthebox.com/machines/PermX?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 06 Jul, 2024  
**Platforms**: HackTheBox  
**Tags**: CMS

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – Apache)  
- HTTP redirect to:  
  - `permx.htb`  
- Subdomain discovered:  
  - `lms.permx.htb`  

---

## Vulnerability Identified
- `lms.permx.htb` running **Chamilo LMS 1.11.24**  
- Version vulnerable to **Remote Code Execution (RCE)**  
- Exploitation allowed command execution as `www-data`  

---

## Initial Access
- Used the Chamilo RCE to obtain a reverse shell  
- Gained access as `www-data`  

---

## Lateral Movement
- Searched for credentials within application files  
- Found database credentials in:  
  - `/var/www/chamilo/app/config/configuration.php`  
- Retrieved valid local user credentials  
- SSH access obtained as user `mtz`  

---

## Privilege Escalation
- `sudo -l` revealed permission to execute a custom ACL script  
- Script allowed modification of file permissions  
- Created a symbolic link pointing to `/etc/passwd`  
- Used the ACL script to grant write permissions  
- Modified `/etc/passwd` to escalate privileges  
- Obtained root access  

---

## Key Takeaways
- Outdated LMS platforms often expose critical RCE vulnerabilities  
- Configuration files frequently contain reusable credentials  
- Custom sudo scripts manipulating ACLs can be dangerous  
- Symbolic link abuse may lead to full privilege escalation  
