# HTB — Hawk

**Official HTB page**: [Hawk](https://app.hackthebox.com/machines/Hawk?tab=play_machine)

**Difficulty**: medium  
**OS**: Linux  
**Release date**: 14 Jul, 2018  
**Platforms**: HackTheBox  
**Tags**: FTP, CMS

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 21 (FTP), 22 (SSH), 80/443 (HTTP/HTTPS), others  
  - Internal web services on uncommon ports 
- FTP anonymous login provided access to encrypted files

## Vulnerability Identified
- Sensitive information stored on an anonymous FTP share
- Encrypted file protected with weak password
- Outdated Drupal installation allowing administrative access

## Initial Access
- Encrypted file retrieved from FTP
- Password cracked using a wordlist attack
- Decrypted file revealed Drupal administrator credentials
- Admin access allowed PHP execution through Drupal modules
- Remote command execution achieved via PHP web shell

## Lateral Movement
- Database credentials discovered in Drupal configuration files
- Credentials reused to access the system via SSH as user `daniel`

## Privilege Escalation
- Process enumeration revealed an H2 database service running as root
- Service was bound to localhost only
- SSH local port forwarding used to access the internal service
- H2 database console allowed execution of system commands
- Root privileges obtained through command execution

## Key Takeaways
- Anonymous FTP access can expose sensitive internal data
- Encrypted files often rely on weak or reused passwords
- CMS administration panels can lead to full compromise
- Configuration files frequently contain reusable credentials
- Internal services should not run as root without strong access controls
- Port forwarding is useful to reach locally bound services

