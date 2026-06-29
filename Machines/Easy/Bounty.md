# HTB — Bounty

**Official HTB page**: https://app.hackthebox.com/machines/Bounty?tab=machine_info

**Difficulty**: Easy  
**OS**: Windows  
**Platform**: HackTheBox  

**Tags**: FileUpload

---

## Initial Enumeration
- Nmap scan revealed a single open port:
  - 80 (HTTP - Microsoft IIS 7.5)
- Directory fuzzing revealed:
  - `transfer.aspx`
- `transfer.aspx` provides a file upload functionality

---

## Vulnerability Identification
- Tested the upload feature with multiple extensions
- File extension filtering accepted `.config` files
- Research on IIS upload vulnerabilities revealed that `web.config` files can be abused for code execution
- Crafted a malicious `web.config` containing Classic ASP code

---

## Initial Access
- Uploaded the malicious `web.config`
- Modified the ASP payload to execute a reverse shell
- Hosted the payload over an SMB share
- Triggered the uploaded file through the web server
- Obtained a reverse shell as the IIS service account

---

## Shell Access
- Initial shell obtained through malicious file upload
- Confirmed code execution on the target

---

## Privilege Escalation
- Enumerated user privileges
- `SeImpersonatePrivilege` was enabled
- Used Juicy Potato to impersonate SYSTEM
- Successfully spawned a SYSTEM shell

---

## Root Access
- Full administrative access obtained via Juicy Potato

---

## Key Takeaways
- File upload functionality should always be tested with uncommon extensions
- IIS can execute malicious `web.config` files when improperly configured
- SMB shares are useful for staging payloads on Windows targets
- `SeImpersonatePrivilege` remains a common Windows privilege escalation vector
