# HTB — Lame

**Official HTB page**: [Lame](https://app.hackthebox.com/machines/1?tab=play_machine)

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 14 Mar, 2017  
**Platforms**: HackTheBox  
**Tags**: Samba

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 21 (FTP – vsftpd 2.3.4)
  - 22 (SSH)
  - 139 / 445 (Samba)
  - 3632 (distccd)
- Anonymous FTP login allowed
- Samba version identified:
  - **Samba 3.0.20-Debian**
- distccd service exposed

---

## Vulnerability Identified
- Multiple potentially vulnerable services:
  - vsftpd 2.3.4
  - distccd
  - Samba
- The correct attack path was:
  - **Samba 3.0.20-Debian**
- Vulnerable to:
  - **CVE-2007-2447**
- Vulnerability allows command execution via Samba username map script injection

---

## Initial Access
- Used publicly available exploit for:
  - Samba 3.0.20 (CVE-2007-2447)
- Achieved remote command execution
- Obtained shell access directly as root

---

## Key Takeaways
- Older Linux services often expose multiple apparent attack paths
- Rabbit holes are common when multiple outdated services are present
- Always verify service versions carefully before choosing exploit path

