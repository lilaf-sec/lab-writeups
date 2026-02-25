# HTB — Cap

**Official HTB page**: [Cap](https://app.hackthebox.com/machines/Cap?tab=machine_info)

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 05 Jun, 2021  
**Platforms**: HackTheBox  
**Tags**: IDOR

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 21 (FTP)
  - 22 (SSH)
  - 80 (HTTP – Gunicorn)
- Web application titled:
  - *Security Dashboard*

---

## Vulnerability Identified
- Web application contained an **IDOR (Insecure Direct Object Reference)**
- Access control was not properly enforced
- By modifying resource identifiers, it was possible to access restricted files
- Retrieved a `.pcap` file from another user session

---

## Initial Access
- Analyzed the exposed `.pcap` file
- Extracted SSH credentials from captured traffic
- Used recovered credentials to authenticate via SSH
- Gained access as user `nathan`

---

## Privilege Escalation
- Enumerated Linux capabilities using `getcap`
- Discovered `/usr/bin/python3.8` had:
  - `cap_setuid`
- This capability allows setting the effective user ID
- Used Python to set UID to 0
- Escalated privileges to root
- Obtained full root access

---

## Key Takeaways
- IDOR vulnerabilities can directly expose sensitive files
- Linux capabilities can be as dangerous as SUID binaries
- `cap_setuid` on interpreters like Python leads to instant root
- Always enumerate file capabilities during privilege escalation

