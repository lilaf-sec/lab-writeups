# HTB — SolidState

**Official HTB page**: [SolidState](https://app.hackthebox.com/machines/SolidState?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 08 Sep, 2017  
**Platforms**: HackTheBox  
**Tags**: python  

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH – OpenSSH 7.4p1 Debian)
  - 25 (SMTP)
  - 80 (HTTP – Apache 2.4.25)
  - 110 (POP3)
  - 119 (NNTP)
  - 4555 (JAMES Remote Administration Tool)
- Port 4555 exposed JAMES Remote Administration Tool 2.3.2
- Default credentials `root:root` allowed administrative access
- Enumerated existing users:
  - james
  - thomas
  - john
  - mindy
  - mailadmin

---

## Vulnerability Identified
- JAMES admin console allowed password reset for any user
- Reset password for user `john`
- Connected to POP3 (port 110) using modified credentials
- Retrieved email containing internal instructions
- Reset password for user `mindy`
- Accessed Mindy’s mailbox via POP3
- Discovered SSH credentials in email

---

## Initial Access
- Connected via SSH as `mindy`
- User was restricted in `rbash`
- Bypassed restricted shell by spawning a new bash session
- Obtained interactive shell access

---

## Privilege Escalation
- Uploaded and executed pspy for process monitoring
- Observed root executing:
  - `python /opt/tmp.py`
- File permissions showed `/opt/tmp.py` was world-writable
- Modified the script to execute privileged command
- Added payload to grant SUID to `/bin/bash`
- Executed script automatically as root
- Spawned root shell via SUID binary

---

## Key Takeaways
- Default credentials on administrative services lead to full compromise
- Mail services often contain sensitive internal credentials
- Restricted shells can frequently be bypassed
- World-writable files executed by root are critical privilege escalation vectors
- Process monitoring tools like pspy are extremely valuable during privesc
