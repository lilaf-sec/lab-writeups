# HTB — Editor

**Official HTB page**: https://app.hackthebox.com/machines/Editor?tab=machine_info

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 02 Aug, 2025  
**Platforms**: HackTheBox  
**Tags**: CMS

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP – nginx)
  - 8080 (HTTP – Jetty)
- Port 80 redirected to:
  - `editor.htb`
- Port 8080 hosted:
  - **XWiki**
- Robots.txt and WebDAV methods exposed
- Jetty version identified:
  - 10.0.20

---

## Vulnerability Identified

- XWiki – CVE-2025-24893
- XWiki instance vulnerable to:
  - **Remote Code Execution**
- Public exploit available
- Vulnerability allowed arbitrary command execution

---

## Initial Access
- Used public exploit for CVE-2025-24893
- Achieved remote code execution
- Obtained shell access on the system
- Discovered SSH credentials for user `oliver`
- Authenticated via SSH as `oliver`

---

## Privilege Escalation

- ndsudo – CVE-2024-32019
- Identified vulnerable SUID binary:
  - `ndsudo`
- Vulnerable to:
  - **CVE-2024-32019**
- Exploited privilege escalation flaw
- Gained root shell
- Obtained full root access

---

## Key Takeaways
- Web applications running on alternate ports often expose critical services
- Searching application directories can reveal stored credentials
