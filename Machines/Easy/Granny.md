# HTB — Granny

**Official HTB page**: [Granny](https://app.hackthebox.com/machines/Granny?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 12 Apr, 2017  
**Platforms**: HackTheBox  
**Tags**: File Upload

---

## Initial Enumeration
- Nmap scan revealed:  
  - 80 (HTTP – Microsoft IIS 6.0)  
- WebDAV enabled with multiple risky HTTP methods  
- Target identified as **Windows Server 2003**  

---

## Vulnerability Identified
- WebDAV allowed file upload to the web root  
- Direct upload of executable extensions was blocked  
- `MOVE` method enabled file rename on the server  

---

## Initial Access
- Uploaded an ASPX webshell as `.txt`  
- Renamed it to `.aspx` using the `MOVE` method  
- Achieved command execution through the webshell  
- Obtained a shell as `NETWORK SERVICE`  

---

## Privilege Escalation
- Checked privileges with `whoami /priv`  
- `SeImpersonatePrivilege` was enabled  

- Abused token impersonation using **Churrasco**  
- Executed a reverse shell as **SYSTEM**  

---

## Key Takeaways
- WebDAV misconfiguration can lead to arbitrary file upload  
- HTTP `MOVE` method can be abused to bypass upload restrictions  
- Legacy Windows systems frequently expose `SeImpersonatePrivilege`  
- Token impersonation remains a reliable path to SYSTEM  
