# HTB — ScriptKiddie

**Official HTB page**: [ScriptKiddie](https://app.hackthebox.com/machines/ScriptKiddie?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 06 Feb, 2021  
**Platforms**: HackTheBox  
**Tags**: FileUpload

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 5000 (HTTP – Werkzeug / Python)  
- Web application identified as a toolkit for generating payloads  

---

## Vulnerability Identified
- The application used **msfvenom** to generate APK payloads  
- Vulnerable to **command injection via malicious APK template**  
- Public exploit available for Metasploit Framework 6.0.11  

---

## Initial Access
- Uploaded a crafted APK template  
- Achieved command execution through msfvenom  
- Obtained a reverse shell as user `kid`  

---

## Pivot
- Discovered another user `pwn`  
- Found a world-readable script in `/home/pwn/` executed periodically  
- Script processed the file `/home/kid/logs/hackers` and launched `nmap` on extracted IPs  

- The script:
  - Used space as field separator
  - Took input starting from the third field
  - Performed no input sanitization
  → Command injection was possible  

- The web application wrote attacker-controlled data to the same log file when invalid input was supplied  

- Injected a reverse shell payload into `/home/kid/logs/hackers`  
- When the script executed, the payload was triggered  
- Gained a shell as user `pwn`  

---

## Privilege Escalation
- User `pwn` had access to **msfconsole** with elevated privileges  
- Spawned an interactive Ruby shell inside Metasploit  
- Executed system commands as root  
- Achieved full system compromise  

---

## Key Takeaways
- Applications wrapping security tools can introduce command injection vulnerabilities  
- Writable files processed by cron jobs are strong privilege escalation vectors  
- Misconfigured Metasploit installations may allow root command execution  
- Always validate user-controlled input passed to system commands  
