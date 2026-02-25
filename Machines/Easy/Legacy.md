# HTB — Legacy

**Official HTB page**: [Legacy](https://app.hackthebox.com/machines/2?tab=play_machine)

**Difficulty**: Easy  
**OS**: Windows XP  
**Release date**: 15 Mar, 2017  
**Platforms**: HackTheBox  
**Tags**: SMB

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 135 (MSRPC)
  - 139 (NetBIOS)
  - 445 (SMB)
- OS identified as:
  - **Windows XP**
- SMB signing disabled
- SMBv2 not supported

---

## Vulnerability Identified
- Additional vulnerability scanning revealed:
  - **MS17-010**
- Legacy Windows XP systems are commonly vulnerable to:
  - **MS08-067 (NetAPI)**
- Target confirmed to be unpatched and exploitable via SMB

---

## Initial Access
- Used public exploit for:
  - **MS08-067 (NetAPI)**
- Successfully achieved remote code execution
- Obtained SYSTEM-level shell access

---

## Key Takeaways
- Windows XP systems are highly vulnerable due to lack of modern security patches
- SMB services are frequent targets for remote exploitation
- MS08-067 remains one of the most classic and reliable Windows exploits

