# HTB — Love

**Official HTB page**: [Love](https://app.hackthebox.com/machines/344?tab=machine_info)

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 01 May, 2021  
**Platforms**: HackTheBox  
**Tags**: SSRF

---

## Initial Enumeration
- Nmap scan revealed multiple services:
  - 80 / 443 (Apache – PHP)
  - 445 (SMB)
  - 3306 (MariaDB)
  - 5985 (WinRM)
- SSL certificate revealed hostname:
  - `staging.love.htb`
- Website on port 80:
  - **Voting System using PHP**

---

## Vulnerability Identified

### Subdomain Discovery
- Added `staging.love.htb` to `/etc/hosts`
- Discovered an additional page:
  - `/beta.php`

### SSRF
- `/beta.php` contained a feature vulnerable to **SSRF**
- Able to force the server to query internal services
- Accessed internal service running on:
  - `localhost:5000`
- Retrieved application credentials

### Authenticated RCE
- The application:
  - **Voting System 1.0**
- Known vulnerability:
  - Authenticated file upload leading to RCE
- Used obtained credentials to authenticate
- Uploaded malicious PHP payload
- Achieved remote code execution
- Obtained reverse shell

---

## Privilege Escalation
- Enumerated Windows registry configuration
- Identified misconfiguration:
  - `AlwaysInstallElevated` enabled in both HKLM and HKCU
- This allows MSI packages to execute with SYSTEM privileges
- Generated malicious MSI payload
- Executed MSI installer
- Escalated privileges to SYSTEM

---

## Key Takeaways
- Subdomains often expose development or staging environments
- SSRF can expose internal-only services and credentials
- Authenticated file upload vulnerabilities can directly lead to RCE
- `AlwaysInstallElevated` is a critical Windows misconfiguration
