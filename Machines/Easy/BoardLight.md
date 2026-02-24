# HTB — BoardLight

**Official HTB page**: https://app.hackthebox.com/machines/BoardLight  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 25 May, 2024  
**Platforms**: HackTheBox  
**Tags**: CMS

---

## Initial Enumeration
- Nmap revealed:
  - 22 (SSH)
  - 80 (HTTP)
- The web server was running Apache on Ubuntu
- The main domain resolved to `board.htb`

---

## Subdomain Enumeration
- Virtual host fuzzing revealed a subdomain:
  - `crm.board.htb`
- The subdomain hosted a Dolibarr instance
- Version identified: Dolibarr 17.0.0

---

## Vulnerability Identified — CVE-2023-30253
- Dolibarr 17.0.0 is vulnerable to authenticated remote code execution
- Public exploit available for the vulnerability
- Exploitation led to remote shell access on the target system

---

## Credential Discovery
- Configuration files were enumerated
- Database credentials were found in `conf.php`
- The recovered password was reused by a system user
- SSH access was obtained as `larissa`

---

## Privilege Escalation
- SUID binaries were enumerated
- The `enlightenment` binary was found with SUID permissions
- The binary was vulnerable to CVE-2022-37706
- Public exploit leveraged to escalate privileges
- Root access was obtained

---

## Key Takeaways
- Virtual host enumeration can expose hidden applications
- Outdated web applications frequently contain critical RCE vulnerabilities
- Configuration files often leak reusable credentials
- SUID binaries must always be audited for known exploits
