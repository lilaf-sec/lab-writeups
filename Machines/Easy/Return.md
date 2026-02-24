# HTB — Return

**Official HTB page**: [Return](https://app.hackthebox.com/machines/Return?tab=machine_info)

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 27 Sep, 2021  
**Platforms**: HackTheBox  
**Tags**: Active Directory

---

## Initial Enumeration
- Nmap scan revealed multiple Active Directory related services:
  - 53 (DNS)
  - 88 (Kerberos)
  - 389 (LDAP)
  - 445 (SMB)
  - 5985 (WinRM)
- Domain identified:
  - `return.local`
- Web server hosted an **IIS** application titled:
  - *HTB Printer Admin Panel*

---

## Vulnerability Identified
- The web application contained a settings panel with:
  - Server Address
  - Server Port
  - Username
  - Password
- Changing the Server Address to an attacker-controlled host caused the application to authenticate to it
- Captured credentials from the outbound authentication attempt

---

## Initial Access
- Reused captured credentials to authenticate remotely
- Established a WinRM session
- Gained access as a domain service account

---

## Privilege Escalation
- The compromised account was member of:
  - `Server Operators`
- This group allows modification and control of certain services
- Direct service creation was restricted
- Enumerated existing services and identified one with sufficient privileges
- Modified the service binary path to execute a custom payload
- Stopped and restarted the service
- Service executed with elevated privileges
- Achieved full administrative access

---

## Key Takeaways
- Applications that authenticate to external services can leak credentials if misconfigured
- Service accounts in AD environments often have elevated privileges
- Membership in `Server Operators` enables service configuration abuse
- Existing services can be hijacked when new service creation is restricted
- Service control abuse is a reliable Windows privilege escalation technique

