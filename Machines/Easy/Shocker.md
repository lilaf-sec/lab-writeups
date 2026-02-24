# HTB — Shocker

**Official HTB page**: https://app.hackthebox.com/machines/Shocker?tab=machine_info

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 30 Sep, 2017  
**Platforms**: HackTheBox  
**Tags**: Shellshock

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 80 (HTTP – Apache)
  - 2222 (SSH)
- Web server running:
  - Apache 2.4.18
- Directory fuzzing revealed:
  - `/cgi-bin/`
- Further enumeration discovered:
  - `/cgi-bin/user.sh`

---

## Vulnerability Identified

### Shellshock
- CGI script (`user.sh`) vulnerable to:
  - **Shellshock (CVE-2014-6271)**
- Bash environment variables were not properly sanitized
- Allowed command injection via HTTP headers

---

## Initial Access
- Injected malicious payload through `User-Agent` header
- Confirmed remote command execution
- Spawned reverse shell
- Gained access as user `shelly`

---

## Privilege Escalation
- `sudo -l` revealed:
  - Ability to run `/usr/bin/perl` as root
- Perl can execute arbitrary commands
- Spawned root shell via Perl execution
- Obtained full root access

---

## Key Takeaways
- Shellshock remains one of the most impactful Bash vulnerabilities
- HTTP headers can be used as injection vectors
- Sudo permissions allowing interpreters (Perl, Python, etc.) lead to instant privilege escalation
