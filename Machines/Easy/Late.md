# HTB — Late

**Official HTB page**: [Late](https://app.hackthebox.com/machines/Late?tab=play_machine)

**Difficulty**: easy  
**OS**: Linux  
**Release date**: 23 Apr, 2022  
**Platforms**: HackTheBox  
**Tags**: [SSTI](/Tags/SSTI.md)

---

## Initial Enumeration
- Nmap revealed:
  - 22 (SSH)
  - 80 (HTTP – nginx, "Late - Best online image tools")
- A subdomain `images.late.htb` offered an **"image to text"** (OCR) conversion service
- The service returned the extracted text rendered inside the page

## Vulnerability Identified
- The OCR result was passed into a server-side template → **Server-Side Template Injection** (Flask / Jinja2)
- Submitting an image containing `{{7*7}}` returned **49**, confirming evaluation
- A service account's SSH login triggered a **world-writable** root-run script

## Initial Access
1. Crafted an image whose text was a Jinja2 payload (instead of plain text)
2. Used the SSTI to read server files, retrieving `/etc/passwd` and a user's **SSH private key**
3. Logged in over SSH as **svc_acc**

## Privilege Escalation
- Enumerated files owned by `svc_acc` → `/usr/local/sbin/ssh-alert.sh`, writable by the user
- With `pspy` confirmed the script runs as **root** on every SSH login (triggered through PAM)
- Appended a command to the script to set the **SUID bit** on `/bin/bash`
- Opened a new SSH session to trigger it, then spawned a root shell with `bash -p`

## Key Takeaways
- User-controlled input rendered through a template engine leads to SSTI → file read and RCE
- `{{7*7}}` is the quickest confirmation test for template injection
- OCR and other indirect input paths are still attacker-controlled
- A root-run script writable by a lower-privileged user is a direct privilege-escalation path
- PAM-triggered login scripts run as root — never leave them writable
