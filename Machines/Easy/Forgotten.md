# HTB — Forgotten

**Official HTB page**: https://app.hackthebox.com/machines/Forgotten?tab=play_machine

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 14 Aug, 2025  
**Platforms**: HackTheBox  
**Tags**: Docker, File upload, SQL

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP – Apache)
- Main page returned **403 Forbidden**
- Directory enumeration revealed:
  - `/survey`
- `/survey` contained an unfinished **LimeSurvey** installation

---

## Exploitation (Installer Abuse)

- LimeSurvey setup wizard allowed custom database configuration
- Application accepted external database endpoints
- Deployed controlled MariaDB instance
- Completed installation process
- Gained administrative access to LimeSurvey

---

## Vulnerability Identified

-  LimeSurvey – CVE-2021-44967
- Version vulnerable to arbitrary file upload via plugin manager
- Uploaded malicious plugin
- Triggered reverse shell
- Gained shell access inside Docker container as `limesvc`

---

## Lateral Movement (Container → Host)

- Enumeration revealed:
  - Running inside Docker container
  - User part of `sudo` group
- Environment variables exposed:
  - `LIMESURVEY_PASS`
- Reused credentials to authenticate to host via SSH
- Achieved low-privileged access on host

---

## Privilege Escalation

- Container user had full sudo privileges inside container
- Identified bind mount shared between container and host:
  - `/var/www/html/survey` ↔ `/opt/limesurvey`
- Copied `/bin/bash` inside shared directory from container
- Set setuid bit on copied binary
- Executed modified binary from host
- Escalated privileges to root

---

## Key Takeaways
- Third-party installers accepting arbitrary database endpoints can be abused
- Container environments must not expose writable host mounts
- Combining root inside container with low-privilege host access enables container escape
