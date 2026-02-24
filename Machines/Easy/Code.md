# HTB — Code

**Official HTB page**: [Code](https://app.hackthebox.com/machines/Code?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 22 Mar, 2025  
**Platforms**: HackTheBox  
**Tags**: Python

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 5000 (HTTP – Gunicorn)  
- Web application identified as a **Python code editor**  

---

## Vulnerability Identified
- The application executes user-supplied Python code inside a restricted environment  
- Python sandbox restrictions were bypassed  
- Achieved **remote command execution** by accessing the `os` module through Python object internals  

---

## Initial Access
- Triggered a reverse shell via the sandbox escape  
- Gained access as the `app` user  

---

## Pivot
- Discovered a SQLite database in `/app/instance`  
- Extracted password hash for user `martin`  
- Cracked the hash and reused credentials to obtain a shell as `martin`  

---

## Privilege Escalation
- `sudo -l` revealed the following allowed command:  
  - `/usr/bin/backy.sh` (NOPASSWD)  
- The script uses a user-controlled JSON configuration file  
- Abuse of path traversal in `directories_to_archive` allowed access to `/root`  
- Backup archive contained sensitive root files including SSH keys and the root flag  

---

## Key Takeaways
- Python sandboxes are often bypassable through object introspection  
- Internal application databases frequently store reusable credentials  
- Sudo permissions on custom scripts must strictly validate user input  
- Backup utilities are a common privilege escalation vector when misconfigured  
