# HTB — Headless

**Official HTB page**: [Headless](https://app.hackthebox.com/machines/Headless?tab=play_machine)

**Difficulty**: easy  
**OS**: Linux  
**Release date**: 23 Mar, 2024  
**Platforms**: HackTheBox  
**Tags**: XSS, Command Injection

---

## Initial Enumeration
- Nmap revealed two open ports:
  - 22 (SSH – OpenSSH 9.2p1)
  - 5000 (HTTP – Werkzeug / Python 3.11)
- The web application showed an "Under Construction" page
- Directory brute force with `gobuster` revealed:
  - `/support` – a contact form
  - `/dashboard` – returned a 500 error without proper authorization
- An `is_admin` cookie was set for regular users

## Vulnerability Identified
- Submitting an XSS payload in the support form triggered a **"Hacking Attempt Detected"** page
- The page stated that a report with the **browser information** (request headers) was sent to the administrators
- The reflected headers were not sanitized → **Blind XSS** through the `User-Agent` header
- The admin dashboard was vulnerable to **command injection** in the `date` parameter

## Initial Access
1. Triggered the detection with an XSS payload in the form and placed a `<script>` tag pointing to an attacker-hosted JavaScript file in the `User-Agent` header
2. When the administrator reviewed the report, the script ran and sent the admin's `is_admin` cookie to a local HTTP server
3. Reused the stolen cookie to access `/dashboard`
4. Intercepted the dashboard request with Burp Suite / Caido and appended a command to the `date` parameter
5. Obtained a reverse shell as the user `dvir`

## Privilege Escalation
- `sudo -l` showed that `dvir` could run `/usr/bin/syscheck` as root without a password
- The `syscheck` script calls `./initdb.sh` through a **relative path** when the process is not running
- Created a malicious `initdb.sh` in the current directory that sets the **SUID bit** on `/bin/bash`
- Ran `sudo /usr/bin/syscheck` → `/bin/bash` became SUID root
- Spawned a root shell with `bash -p`

## Key Takeaways
- Admin-side reports and logs that render user-controlled data are a classic Blind XSS target
- Headers like `User-Agent` are user input too and must be sanitized
- Cookies without the `HttpOnly` flag can be stolen through XSS → admin session hijacking
- Passing user input to system commands without validation leads to remote code execution
- Scripts run through sudo must never call binaries or scripts by relative path
