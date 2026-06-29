# HTB — Active

**Official HTB page**: [Active](https://app.hackthebox.com/machines/Active?tab=machine_info)

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 28 Jul, 2018  
**Platforms**: HackTheBox  
**Tags**: SMB, ActiveDirectory, Kerberos 

---

## Initial Enumeration
- Nmap scan revealed multiple AD services:
  - 53 (DNS)
  - 88 (Kerberos)
  - 135/139/445 (SMB)
  - 389/3268 (LDAP)
- Domain identified:
  - `active.htb`
- SMB signing enabled
- Null session allowed enumeration

---

## Vulnerability Identified
- SMB share access with anonymous authentication
- Found accessible share:
  - `Replication`
- Extracted `Groups.xml` from SYSVOL
- File contained GPP credentials (cpassword)

---

## Initial Access
- Decrypted GPP password using `gpp-decrypt`
- Obtained credentials for user `SVC_TGS`
- Enumerated domain users and shares via SMB
- Accessed `Users` share
- Retrieved user flag from desktop directory

---

## Exploitation (Kerberoasting)
- Performed Kerberoasting attack using:
  - `GetUserSPNs.py`
- Retrieved TGS ticket for `Administrator`
- Cracked ticket using hashcat
- Obtained Administrator credentials

---

## Privilege Escalation
- Verified SMB access as Administrator
- Executed remote command execution using `psexec`
- Spawned SYSTEM shell on domain controller

---

## Key Takeaways
- GPP XML files can expose domain credentials
- SYSVOL shares are critical during AD enumeration
- Kerberoasting allows offline cracking of service accounts
- Weak service account passwords lead to full domain compromise
- Psexec provides instant SYSTEM-level access once admin creds are obtained
