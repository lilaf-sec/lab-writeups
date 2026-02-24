# HTB — Netmon

**Official HTB page**: [Netmon](https://app.hackthebox.com/machines/Netmon?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 02 Mar, 2019  
**Platforms**: HackTheBox  
**Tags**: FTP

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 21 (FTP – Anonymous access allowed)  
  - 80 (HTTP – PRTG Network Monitor)  
  - 135 / 139 / 445 (SMB)  
  - 5985 (WinRM)  
- Anonymous FTP access exposed the filesystem  

---

## Initial Access
- Retrieved `user.txt` via anonymous FTP  
- Navigated to:  
  - `/ProgramData/Paessler/PRTG Network Monitor/`  
- Found `PRTG Configuration.old.bak`  
- Backup file contained **PRTG administrative credentials**  
- Password required a year adjustment to match the current configuration  
- Authenticated to the PRTG web interface as **System Administrator**

---

## Vulnerability Identified
- PRTG version `18.1.37.13946` vulnerable to **authenticated RCE**  
- Command execution possible through the **notification management feature**

---

## Privilege Escalation
- Used the RCE to create a new local administrator user  
- Gained administrative privileges on the system  
- Connected using **WinRM** and obtained full access  

---

## Key Takeaways
- Anonymous FTP can expose sensitive configuration files  
- Backup files often contain valid credentials  
- Password patterns based on years are predictable  
- Authenticated features in monitoring solutions may lead to RCE  
- WinRM provides stable post-exploitation access on Windows targets  
