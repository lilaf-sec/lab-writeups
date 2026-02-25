# HTB — Blue

**Official HTB page**: [Blue](https://app.hackthebox.com/machines/Blue?tab=machine_info)

**Difficulty**: Easy  
**OS**: Windows 7  
**Release date**: 28 Jul, 2017  
**Platforms**: HackTheBox  
**Tags**: SMB

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 135 (MSRPC)
  - 139 (NetBIOS)
  - 445 (SMB)
- OS identified:
  - Windows 7 Professional SP1
- SMB signing enabled but not required

---

## Vulnerability Identified
- SMB vulnerability scanning revealed:
  - **MS17-010**
- System vulnerable to:
  - **EternalBlue**
- Unpatched Windows 7 system exposed via SMB

---

## Exploitation
- Used public exploit for:
  - MS17-010 (EternalBlue)
- Achieved remote code execution
- Obtained SYSTEM-level shell access directly

---

## Key Takeaways
- Windows 7 SP1 systems are highly vulnerable if unpatched
- MS17-010 remains one of the most impactful SMB vulnerabilities
- Always test SMB services on legacy Windows systems
- Patch management is critical in Windows environments
