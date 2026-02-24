# HTB — Keeper

**Official HTB page**: [Keeper](https://app.hackthebox.com/machines/Keeper?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 20 Jan, 2026  
**Platforms**: HackTheBox  
**Tags**: KeePass

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – Nginx)  
- Web application presented a login panel  

---

## Vulnerability Identified
- Default credentials allowed access to the application  
- User management section exposed valid system credentials  
- Reused credentials to obtain SSH access as a low-privileged user  

---

## Privilege Escalation

- KeePass Memory Dump
- Discovered a ZIP archive containing:  
  - KeePass database (`.kdbx`)  
  - Process memory dump (`.dmp`)  

- The dump was vulnerable to **CVE-2023-32784**  
- Extracted candidate master password from memory  

-  KeePass Database
- Recovered the correct master password  
- Accessed stored credentials inside the vault  
- Retrieved a **PuTTY private key**  

- Root Access
- Converted the PuTTY key to OpenSSH format  
- Logged in via SSH as **root**  

---

## Key Takeaways
- Default credentials remain a critical security risk  
- Password reuse between applications and system accounts leads to full compromise  
- KeePass memory dumps can expose the master password (CVE-2023-32784)  
- Credential vaults often contain high-value assets such as SSH private keys  
