# HTB — Backdoor

**Official HTB page**: https://app.hackthebox.com/machines/Backdoor?tab=machine_info

**Difficulty**: Easy  
**OS**: Linux  
**Platform**: HackTheBox  

**Tags**: CMS, LFI

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP - WordPress)
  - 1337 (unknown service)
- Web service identified as WordPress 5.8.1
- Plugin enumeration revealed:
  - `ebook-download` plugin

---

## Vulnerability Identification
- Plugin `ebook-download` is vulnerable to directory traversal
- Parameter `ebookdownloadurl` allows arbitrary file reading
- Confirmed LFI via:
  - `/etc/passwd`
- Vulnerable endpoint:
  - `/wp-content/plugins/ebook-download/filedownload.php`

---

## Exploitation (LFI → Process Enumeration)
- LFI used to enumerate `/proc` filesystem
- Brute force on `/proc/[PID]/cmdline` revealed running processes
- Identified process running:
  - `gdbserver` bound on port `1337`

---

## Initial Access (RCE via gdbserver)
- gdbserver service exposed on port 1337
- Public exploit available for remote command execution
- Generated reverse shell payload using msfvenom
- Exploit executed against target gdbserver instance
- Reverse shell obtained successfully

---

## Shell Access
- Initial user shell achieved via gdbserver exploitation
- Access confirmed on target system

---

## Privilege Escalation
- Found SUID binary:
  - `/usr/bin/screen`
- Root screen session detected:
  - screen session running under root context
- Background process continuously creating root screen session

---

## Exploitation (screen hijack)
- Attached to root screen session using:
  - `screen -x root/`
- Successfully gained root shell via session hijacking

---

## Root Access
- Full root access obtained through screen session attachment

---

## Key Takeaways
- WordPress plugins remain a major attack surface
- LFI can be escalated into process discovery via `/proc`
- Hidden services often run internally (gdbserver in this case)
- Misconfigured SUID binaries like `screen` can lead to root compromise
