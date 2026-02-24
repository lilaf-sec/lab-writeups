# HTB — CozyHosting

**Official HTB page**: [CozyHosting](https://app.hackthebox.com/machines/CozyHosting?tab=play_machine)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 02 Sep, 2023  
**Platforms**: HackTheBox  
**Tags**: Other

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – Nginx)  
- Redirect to virtual host `cozyhosting.htb`  
- Web application built with **Spring Boot**  

---

## Vulnerability Identified
- Directory brute force revealed `/actuator` endpoints  
- `/actuator/sessions` exposed active session data  
- Reused session cookie to access the admin panel  
- Command injection in the host management feature  
- `${IFS}` used to bypass space filtering  
- Achieved remote command execution via crafted payload  

---

## Initial Access
- Triggered a reverse shell through command injection  
- Gained access as the `app` user  

---

## Pivot
- Retrieved the application JAR file from the system  
- Extracted and analyzed its contents  
- Discovered database credentials in `application.properties`  
- Connected to the local **PostgreSQL** database  
- Extracted password hashes from the `users` table  
- Cracked the hash and reused credentials to obtain SSH access as `josh`  

---

## Privilege Escalation
- `sudo -l` revealed the following allowed command:  
  - `/usr/bin/ssh` (root)  

- Abused `ProxyCommand` option to spawn a root shell  

---

## Key Takeaways
- Exposed Spring Boot Actuator endpoints can lead to full compromise  
- Session leakage enables authentication bypass  
- Application artifacts often contain hardcoded credentials  
- Database access frequently provides reusable system credentials  
- Sudo permissions on SSH can be abused for instant root access  
