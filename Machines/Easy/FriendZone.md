# HTB — FriendZone

**Official HTB page**: [FriendZone](https://app.hackthebox.com/machines/FriendZone?tab=play_machine)

**Difficulty**: easy  
**OS**: Linux  
**Release date**: 09 Feb, 2019  
**Platforms**: HackTheBox  
**Tags**: LFI, SMB, DNS, Python

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 21 (FTP), 53 (DNS), 80/443 (HTTP/HTTPS), 139/445 (SMB), others  
- DNS zone transfer (AXFR) was allowed → revealed internal subdomains like `administrator1.friendzone.red`  
- SMB anonymous shares accessible → listed and read several shares  
- FTP anonymous login possible but no interesting files

## Vulnerability Identified
- DNS misconfiguration: full zone transfer allowed subdomain enum  
- SMB: anonymous read on sensitive shares leaking credentials  
- One SMB share writable (upload possible)  
- Web app on subdomain vulnerable to **Local File Inclusion (LFI)**

## Initial Access
1. Grabbed creds from SMB share  
2. Used creds to access `administrator1.friendzone.red` web panel  
3. Exploited **LFI** to read files from the writable SMB share  
4. Uploaded a simple PHP webshell via the writable directory  
5. Triggered RCE by including the webshell via LFI  

## Privilege Escalation
- Credentials for user `friend` found inside `/var/www`
- `pspy` revealed a Python script executed periodically as root
- The script imported the `os` library
- Write permissions were available in `/usr/lib`
- Python library hijacking was performed by modifying `os.py`

## Key Takeaways
- DNS zone transfer can expose internal infrastructure
- SMB shares often leak credentials or allow file uploads
- LFI can be combined with writable directories for code execution
- Monitoring running processes helps identify privilege escalation paths
- Python library hijacking is possible when scripts run as root with insecure permissions
