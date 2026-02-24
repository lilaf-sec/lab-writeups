# HTB — Stratosphere

**Official HTB page**: [Stratosphere](https://app.hackthebox.com/machines/Stratosphere?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 03 Mar, 2018  
**Platforms**: HackTheBox  
**Tags**: python  

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH – OpenSSH 7.9p1 Debian)
  - 80 (HTTP – Apache Tomcat)
  - 8080 (HTTP – Apache Tomcat)
- Both 80 and 8080 hosted the same Tomcat application
- Fuzzing revealed:
  - `/Monitoring/example/Welcome.action`

---

## Vulnerability Identified
- The endpoint `/Monitoring/example/Welcome.action` was vulnerable to **CVE-2017-5638**
- Vulnerability affected Apache Struts (OGNL injection)
- Allowed unauthenticated remote command execution

---

## Initial Access
- Exploited the vulnerability using a public Struts exploit
- Achieved remote command execution
- Located `db_connect` file containing database credentials
- Enumerated MySQL databases
- Extracted credentials from `users.accounts` table
- Reused discovered password for SSH access
- Logged in as `richard`

---

## Privilege Escalation
- `sudo -l` revealed:
  - `/usr/bin/python* /home/richard/test.py` (NOPASSWD)
- The script imported the `hashlib` library
- Python executed from the current working directory first
- Created a malicious `hashlib.py`
- Injected command to grant SUID to `/bin/bash`
- Executed:
  - `sudo /usr/bin/python /home/richard/test.py`
- Triggered Python Library Hijacking
- Spawned root shell via SUID binary

---

## Key Takeaways
- Apache Struts CVE-2017-5638 provides full unauthenticated RCE
- Configuration files often contain reusable credentials
- Database enumeration can lead to system access
- Sudo permissions on Python scripts are dangerous
- Python Library Hijacking is a reliable privilege escalation technique
