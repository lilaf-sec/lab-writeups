# HTB — Photobomb

**Official HTB page**: [Photobomb](https://app.hackthebox.com/machines/Photobomb?tab=play_machine)

**Difficulty**: easy  
**OS**: Linux  
**Release date**: 08 Oct, 2022  
**Platforms**: HackTheBox  
**Tags**: Command Injection, Path Hijacking

---

## Initial Enumeration
- Nmap revealed:
  - 22 (SSH)
  - 80 (nginx, redirect to `photobomb.htb`)
- The site linked to a password-protected area (`/printer`)
- Reviewing the source, a referenced JavaScript file (`photobomb.js`) **pre-populated Basic Auth credentials** in a comment left by a developer

## Vulnerability Identified
- The leaked credentials granted access to the `/printer` download feature
- The download endpoint passed the `filetype` parameter into a system command → **command injection**

## Initial Access
1. Extracted the Basic Auth credentials from `photobomb.js`
2. Accessed `/printer`, which let users download images at a chosen resolution
3. Intercepted the download request and injected a command into the `filetype` field
4. Triggered a reverse shell → foothold as the user **wizard**

## Privilege Escalation
- `sudo -l` showed `wizard` could run `/opt/cleanup.sh` as **root** with **`SETENV`** (NOPASSWD)
- The script called `find` (among others) **without an absolute path**, and `SETENV` allowed overriding `PATH`
- Ran the script with a controlled `PATH` pointing to a malicious `find` → executed as root
- Obtained a root shell

## Key Takeaways
- Credentials in client-side JavaScript are visible to anyone reading the source
- User-controlled parameters passed to shell commands lead to command injection
- `sudo` with `SETENV` is dangerous: it lets the caller override `PATH` for the target script
- Scripts run as root must use absolute paths for every command they call
- Combine the two and a limited sudo rule becomes a full root compromise
