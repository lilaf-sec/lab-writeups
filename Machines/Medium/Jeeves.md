# HTB — Jeeves

**Official HTB page**: [Jeeves](https://app.hackthebox.com/machines/Jeeves?tab=play_machine)

**Difficulty**: medium    
**OS**: Windows  
**Release date**: 11 Nov, 2017  
**Platforms**: HackTheBox  
**Tags**: KeePass, Pass-the-hash,  NTLM

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 80/5000 (HTTP), others  
- `askjeeves` directory discovered on port 5000

## Vulnerability Identified
Web console allowing Groovy script execution

## Initial Access
Reverse shell obtained through the web console

## Privilege Escalation
KeePaas database (.kdbx) discovered.

- Database cracked to retrieve credentials
- LM:NTLM hash extracted
- Pass-the-hash technique used to authenticate as Administrator

## Key Takeaways
- Web consoles can allow arbitrary code execution
- Script engines (Groovy, Python, PHP) are often dangerous if exposed
- KeePass databases may contain high-privilege credentials
- Pass-the-hash is a common Windows privilege escalation technique
