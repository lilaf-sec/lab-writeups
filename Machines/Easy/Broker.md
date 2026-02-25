# HTB — Broker

**Official HTB page**: [Broker](https://app.hackthebox.com/machines/Broker?tab=play_machine)

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 05 Nov, 2023  
**Platforms**: HackTheBox  
**Tags**: Deserialization

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP – Nginx)
  - 61616 (Apache ActiveMQ)
- Apache ActiveMQ version identified:
  - 5.15.15

---

## Vulnerability Identified

### Apache ActiveMQ – CVE-2023-46604
- Version 5.15.15 vulnerable to:
  - **CVE-2023-46604**
- Vulnerability caused by unsafe deserialization
- ActiveMQ did not properly validate deserialized classes
- Led to unauthenticated remote code execution

---

## Initial Access
- Used publicly available exploit for CVE-2023-46604
- Hosted malicious XML payload remotely
- Exploit triggered remote class loading via Spring framework
- Achieved remote code execution
- Gained shell access as user `activemq`

---

## Privilege Escalation
- `sudo -l` revealed:
  - User `activemq` could run `/usr/sbin/nginx` as root
- Nginx allowed loading custom configuration files
- Created malicious Nginx configuration:
  - Worker processes running as root
  - Document root set to `/`
  - WebDAV `PUT` method enabled
- Started Nginx with custom configuration
- Uploaded SSH public key into:
  - `/root/.ssh/authorized_keys`
- Authenticated via SSH as root
- Obtained full root access

---

## Key Takeaways
- Deserialization vulnerabilities frequently lead to unauthenticated RCE
- Sudo permissions allowing custom service configuration are extremely dangerous
- WebDAV `PUT` method can be abused for arbitrary file writes

