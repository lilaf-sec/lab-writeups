# HTB — BountyHunter

**Official HTB page**: https://app.hackthebox.com/machines/BountyHunter?tab=machine_info

**Difficulty**: Easy  
**OS**: Linux  
**Platform**: HackTheBox  

**Tags**: XXE, Python

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP)
- Directory fuzzing revealed several endpoints:
  - `portal.php`
  - `db.php`
- `portal.php` exposed a bug report submission form

---

## Vulnerability Identification
- Intercepting the request showed the submitted data was:
  - URL encoded
  - Base64 encoded
  - XML formatted
- Decoding the request revealed an XML document
- The endpoint was vulnerable to **XXE**
- Confirmed arbitrary file read by accessing:
  - `/etc/passwd`
- Retrieved the source of `db.php` using PHP filters
- Source code contained database credentials

---

## Initial Access
- `/etc/passwd` revealed the user `development`
- Database credentials were reused for SSH
- Successfully authenticated as `development`

---

## Privilege Escalation
- Enumerated sudo permissions
- User could execute:
  - `/usr/bin/python3.8 /opt/skytrain_inc/ticketValidator.py`
- Source code analysis revealed unsafe use of Python's `eval()` function
- The script validates Markdown (`.md`) ticket files

---

## Exploitation (Python eval)
- Crafted a malicious Markdown ticket
- Injected Python code into the ticket validation field
- `eval()` executed arbitrary commands as root
- Spawned a root shell through command execution

---

## Root Access
- Root shell obtained by abusing unsafe Python `eval()` usage

---

## Key Takeaways
- XML endpoints should always be tested for XXE vulnerabilities
- PHP filters are useful for retrieving application source code
- Credential reuse frequently leads to initial access
- Unsafe use of Python `eval()` can result in arbitrary code execution
- Reviewing privileged scripts is essential during Linux privilege escalation
