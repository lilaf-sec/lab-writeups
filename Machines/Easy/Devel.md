# HTB — Devel

**Official HTB page**: [Devel](https://app.hackthebox.com/machines/3?tab=machine_info)

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 15 Mar, 2017  
**Platforms**: HackTheBox  
**Tags**: FTP 

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 21 (FTP – Microsoft ftpd)
  - 80 (HTTP – IIS 7.5)
- Anonymous FTP login was allowed
- FTP directory contents matched the IIS web root
- Web server running:
  - Microsoft IIS 7.5

---

## Vulnerability Identified
- Anonymous FTP upload was enabled
- Uploaded files were accessible via the web server
- IIS executed `.asp` / `.aspx` files
- This allowed web shell upload and execution

---

## Initial Access
- Uploaded an ASPX web shell via FTP
- Accessed the uploaded file through the browser
- Achieved remote command execution
- Uploaded a reverse shell binary
- Gained interactive shell access

---

## Privilege Escalation
- System enumeration revealed:
  - Windows version 6.1.7600 (unpatched)
- Identified vulnerability:
  - **MS11-046**
- Executed public exploit for MS11-046
- Escalated privileges to SYSTEM
- Obtained full administrative access

---

## Key Takeaways
- Anonymous FTP access combined with executable web roots leads to instant RCE
- Always verify whether uploaded files are executed server-side
