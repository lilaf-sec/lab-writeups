# HTB — Grandpa

**Official HTB page**: [Grandpa](https://app.hackthebox.com/machines/Grandpa?tab=play_machine)  

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 12 Apr, 2017  
**Platforms**: HackTheBox  
**Tags**: File upload

---

## Initial Enumeration
- Nmap scan revealed:  
  - 80 (HTTP – Microsoft IIS 6.0)  
- WebDAV enabled with multiple risky HTTP methods:  
  - PUT, MOVE, PROPFIND, SEARCH, LOCK, UNLOCK  
- Target identified as **Windows Server 2003**  

---

## Vulnerability Identified
- IIS 6.0 vulnerable to **CVE-2017-7269**  
- WebDAV `ScStoragePathFromUrl` buffer overflow allows remote code execution  

---

## Initial Access
- Exploited the vulnerability using:  
  - Metasploit module `iis_webdav_scstoragepathfromurl`  
  - Public Python exploit  
- Obtained a shell as `NETWORK SERVICE`  

---

## Privilege Escalation
- Checked privileges with `whoami /priv`  
- `SeImpersonatePrivilege` was enabled  
- Uploaded:
  - `churrasco.exe`
  - reverse shell payload  
- Abused token impersonation to execute code as **SYSTEM**  

---

## Key Takeaways
- Legacy IIS versions are highly vulnerable when WebDAV is exposed  
- Public exploits provide quick and reliable RCE on unpatched systems  
- `SeImpersonatePrivilege` is a common and powerful Windows privilege escalation vector  
- Token impersonation attacks lead directly to SYSTEM access  
