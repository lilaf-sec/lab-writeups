# HTB — Knife

**Official HTB page**: [Knife](https://app.hackthebox.com/machines/Knife?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 22 May, 2021  
**Platforms**: HackTheBox  
**Tags**: Other

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – Apache)  
- Web server running **PHP 8.1.0-dev**  

---

## Vulnerability Identified
- PHP version `8.1.0-dev` is vulnerable to the **backdoor RCE**  
- Confirmed via HTTP response headers  

---

## Initial Access
- Used public exploit to trigger command execution  
- Obtained a shell as user `james`  

---

## Privilege Escalation
- `sudo -l` revealed the following allowed command:  
  - `/usr/bin/knife` (root)  
- Abused knife’s Ruby execution feature to spawn a root shell  

---

## Key Takeaways
- Exposed development versions in production can lead to instant compromise  
- Version disclosure in HTTP headers is highly valuable  
- Sudo permissions on scripting frameworks often allow shell escape  
- Always check GTFOBins for privileged binaries  
