# HTB — Alert

**Official HTB page**: [Alert](https://app.hackthebox.com/machines/Alert)

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 23 Nov, 2024
**Platforms**: HackTheBox  
**Tags**: XSS, File upload

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP – Apache)
- The web server redirected to the domain:
  - `alert.htb`
- The website allowed users to upload, view, and share Markdown files
- The “About Us” page mentioned that an administrator reviews submitted messages

---

## Web Enumeration
- Directory enumeration revealed:
  - `messages`
  - `uploads`
  - `css`
- A `messages.php` endpoint was discovered
- Subdomain enumeration revealed:
  - `statistics.alert.htb`
- The `statistics` subdomain required HTTP authentication

---

## Vulnerability Identified — XSS
- The application rendered user-uploaded Markdown files without proper sanitization
- A Cross-Site Scripting (XSS) vulnerability was confirmed
- JavaScript execution was possible when an uploaded Markdown file was viewed
- The Markdown sharing feature allowed sending links to the administrator
- This made it possible to target the administrator via stored XSS

---

## Foothold — Arbitrary File Read
- A malicious payload was crafted to request internal resources
- The payload retrieved the content of `messages.php`
- The response was exfiltrated and decoded
- The `file` parameter in `messages.php` was identified as vulnerable
- Directory traversal allowed reading arbitrary files on the system
- The configuration revealed the location of the `.htpasswd` file for the `statistics` subdomain
- The `.htpasswd` file was retrieved through the same Arbitrary File Read vulnerability
- The Apache `$apr1$` hash was extracted
- The hash was cracked offline
- Valid credentials for user `albert` were obtained

---

## Initial Access
- The recovered credentials allowed authentication to:
  - `statistics.alert.htb`
- The same credentials were valid for SSH access
- SSH access was established as user `albert`
- The user flag was retrieved from the home directory

---

## Privilege Escalation
- Local enumeration revealed that port 8080 was listening internally
- Port forwarding exposed a Website Monitor service
- Process monitoring revealed that `/opt/website-monitor/monitor.php` was executed regularly
- The script was executed as root via a scheduled task
- The script included a `configuration.php` file
- The `configuration.php` file was writable by the `management` group
- User `albert` was a member of the `management` group
- The configuration file was modified to execute arbitrary PHP code
- The modification resulted in setting the SUID bit on `/bin/bash`
- Executing `/bin/bash` with preserved privileges provided a root shell
- The root flag was retrieved from `/root/root.txt`

---

## Key Takeaways
- Markdown rendering without proper sanitization leads to stored XSS
- Stored XSS can be leveraged to target privileged users
- Arbitrary File Read vulnerabilities can expose sensitive configuration files
- Group misconfigurations on included PHP files can lead to privilege escalation
- SUID abuse remains a classic and reliable Linux privilege escalation technique
