# HTB — TwoMillion

**Official HTB page**: [TwoMillion](https://app.hackthebox.com/machines/TwoMillion?tab=play_machine)

**Difficulty**: easy  
**OS**: Linux  
**Release date**: 07 Jun, 2023    
**Platforms**: HackTheBox  
**Tags**: ApiAbuse

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH), 80 (HTTP)  
- Web application hosted on a custom domain
- Registration system based on invite codes
- Public API endpoints discovered

## Vulnerability Identified
- Invite code generation exposed through API
- Weak access control on administrative API endpoints
- Insecure role management allowing privilege escalation

## Initial Access
- Invite code generated through public API endpoint
- User account successfully registered
- Administrative API endpoint lacked proper role validation
- User role modified to administrator

## Remote Code Execution
- Administrative API endpoint executed system commands
- Insufficient input validation allowed command injection
- Command execution achieved through backend application logic
- Interactive shell obtained through command execution

## Lateral Movement
- Administrator password recovered from environment variables

## Privilege Escalation
- System mail revealed information about a known kernel vulnerability
- Vulnerable OverlayFS implementation identified
- Local privilege escalation performed using public vulnerability
- Root access obtained

## Key Takeaways
- API endpoints must enforce strict authorization checks
- Client-side role control is never trustworthy
- Environment configuration files often contain sensitive credentials
- System logs and mail can leak critical security information
- Kernel vulnerabilities can provide full system compromise
