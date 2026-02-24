# HTB — Devzat

**Official HTB page**: [Devzat](https://app.hackthebox.com/machines/Devzat?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 16 Oct, 2021  
**Platforms**: HackTheBox  
**Tags**: LFI

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP)
  - 8000 (Custom SSH service)
- Port 80 redirected to `devzat.htb`
- Subdomain enumeration revealed:
  - `pets.devzat.htb`
- Port 8000 hosted an SSH-based chat service
  - Connection required enabling legacy `ssh-rsa` support
- The chat service exposed various commands but did not provide initial access

---

## Vulnerability Identified
- The application hosted on `pets.devzat.htb` was vulnerable to **command injection**
- Injection was possible via the `species` parameter
- A publicly accessible `.git` directory exposed the source code and confirmed the vulnerability

---

## Initial Access
- Exploited the command injection to achieve remote command execution
- Reverse shell obtained as a low-privileged user

---

## Pivot / Internal Services
- Internal enumeration revealed docker-proxy forwarding local services
- Port forwarding was required to access localhost-only services
- Forwarded ports revealed:
  - Internal HTTP service with exposed
  - InfluxDB service running locally
  - A second SSH chat service only accessible internally

---

## Lateral Movement
- InfluxDB was vulnerable to **CVE-2019-20933**
- The vulnerability allowed extraction of stored database credentials
- Discovered valid user credentials from the InfluxDB instance

---

## Privilege Escalation
- Found a backup archive inside `/var/backups/`
- The archive contained source code and internal secrets
- Discovered the password for an internal chat command
- The internal chat service exposed a `/file` command vulnerable to directory traversal
- File read vulnerability allowed access to sensitive root files
- Retrieved root SSH key and gained full root access

---

## Key Takeaways
- Exposed `.git` directories can leak full application source code
- Port forwarding is essential for accessing internal-only applications
- InfluxDB CVE-2019-20933 allows credential extraction without authentication
- Backup archives commonly contain secrets and sensitive source code
- Directory traversal/LFI can directly lead to root compromise
