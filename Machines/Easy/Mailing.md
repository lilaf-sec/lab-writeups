# HTB — Mailing

**Official HTB page**: [Mailing](https://app.hackthebox.com/machines/Mailing?tab=play_machine)

**Difficulty**: easy  
**OS**: Windows  
**Release date**: 04 May, 2024  
**Platforms**: HackTheBox  
**Tags**: [LFI](/Tags/LFI.md), [NTLM](/Tags/NTLM.md)

---

## Initial Enumeration
- Nmap revealed a Windows mail server:
  - 80 (HTTP – IIS, redirect to `mailing.htb`)
  - 25 / 587 / 465 (SMTP) and 110 / 143 / 993 (POP3 / IMAP) served by **hMailServer**
  - 135 / 139 / 445 (RPC / SMB)
- The website offered a downloadable PDF through a `download.php?file=` parameter

## Vulnerability Identified
- The `file` parameter was vulnerable to **Local File Inclusion** via path traversal (`../../../../`)
- Reading `hMailServer.ini` exposed configuration details and password hashes
- The mail client in use was vulnerable to **CVE-2024-21413** (Microsoft Outlook RCE), which can coerce an **NTLM authentication** to an attacker-controlled host

## Initial Access
1. Confirmed the LFI by reading `C:\Windows\System32\drivers\etc\hosts`
2. Enumerated sensitive paths and retrieved the hMailServer configuration
3. Determined the likely patch window by checking an image's metadata, pointing to a **2024** CVE
4. Ran the **CVE-2024-21413** PoC against SMTP to send a crafted email to a user, coercing a connection to a local **SMB server**
5. Captured a **NetNTLMv2** hash for the user **maya** and cracked it offline
6. Logged in as **maya** over WinRM (`Pwn3d!`)

## Privilege Escalation
- Found **LibreOffice** installed and read `version.ini` → version **7.4.0.1**, vulnerable to **CVE-2023-2255**
- Built a malicious document with the public exploit that executes a command when opened
- Had the document processed by the privileged account → command executed as the higher-privileged user, leading to full compromise

## Key Takeaways
- Unvalidated file-download parameters lead to arbitrary file read via path traversal
- Config files like `hMailServer.ini` frequently store reusable secrets
- CVE-2024-21413 turns a single email into a coerced NTLM authentication
- NetNTLMv2 hashes can be relayed or cracked offline
- Outdated desktop software (LibreOffice) is a common local privilege-escalation vector
