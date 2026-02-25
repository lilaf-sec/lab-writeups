# HTB — Sau

**Official HTB page**: [Sau](https://app.hackthebox.com/machines/Sau?tab=play_machine)

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 08 Jul, 2023
**Platforms**: HackTheBox  
**Tags**: SSRF

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 55555 (HTTP – Request Baskets)
- Port 80 appeared filtered
- Web service identified:
  - **Request Baskets v1.2.1**

---

## Vulnerability Identified

-  Request Baskets – CVE-2023-27163
- Version 1.2.1 vulnerable to:
  - **Server-Side Request Forgery (SSRF)**
- Vulnerability located in:
  - `/api/baskets/{name}`
- Allowed forwarding internal HTTP requests
- Enabled access to localhost services

---

## Pivot (Internal Service Discovery)
- Used SSRF to forward requests to:
  - `127.0.0.1:80`
- Discovered internal web application:
  - **Maltrail v0.53**
- Version vulnerable to:
  - Unauthenticated OS Command Injection

---

## Initial Access
- Leveraged public exploit for Maltrail
- Achieved remote command execution
- Gained shell access as user `puma`

---

## Privilege Escalation
- `sudo -l` revealed:
  - User `puma` could run `systemctl status trail.service` as root
- System running:
  - Systemd 245
- Misconfiguration allowed pager escape via `less`
- Executing `!/bin/bash` inside pager spawned root shell

---

## Key Takeaways
- SSRF can expose internal-only services
- Maltrail v0.53 vulnerable to unauthenticated command injection
- Sudo permissions involving `systemctl` can lead to privilege escalation
- Pager escapes (`less`) remain a powerful local privilege escalation technique
