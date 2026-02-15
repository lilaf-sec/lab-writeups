# HTB — Editorial

**Official HTB page**: [Editorial](https://app.hackthebox.com/machines/Editorial?tab=play_machine)

**Difficulty**: easy 
**OS**: Linux
**Release date**: 15 Jun, 2024  
**Platforms**: HackTheBox  
**Tags**: SSRF, API Abuse

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH), 80 (HTTP)  
- Web application with file upload functionality
- Redirection to virtual host discovered

## Vulnerability Identified
A Server-Side Request Forgery (SSRF) vulnerability was found in the upload feature.

The SSRF allowed:
- internal port discovery
- access to internal API endpoints

## Initial Access
Internal API responses exposed sensitive application data.

Credentials were discovered within an internal endpoint, allowing SSH access
to the system as a low-privileged user.

## Lateral Movement
A local Git repository was discovered on the system.

Reviewing commit history revealed:
- hardcoded credentials
- environment misconfiguration between development and production

This allowed access to a higher-privileged user account.

## Privilege Escalation
The privileged user was allowed to execute a Python script as root.

The script relied on a vulnerable version of a third-party Python package.

By abusing this dependency misconfiguration, command execution as root
was achieved.

## Key Takeaways
- SSRF can lead to full compromise when internal services are exposed
- Git repositories may leak credentials through commit history
- Third-party Python libraries can introduce critical privilege escalation paths
- Always review sudo permissions involving scripts and external libraries


