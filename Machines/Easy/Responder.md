# HTB — Responder

**Official HTB page**: [Responder](https://app.hackthebox.com/machines/Responder)

**Difficulty**: Very Easy 
**OS**: Windows  
**Release date**: 06 Apr, 2022
**Platforms**: HackTheBox  
**Tags**: LFI, SMB, NTLM abuse

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 80 (HTTP – Apache)
  - 5985 (WinRM)
- Web server was identified as a Windows-hosted Apache instance
- Accessing the target IP redirected the browser to a hostname:
  - `unika.htb`
- This indicated the use of name-based virtual hosting

---

## Web Enumeration
- After resolving the hostname locally, the website loaded successfully
- The web application was a basic business landing page
- A language selection option was available (EN / FR)
- Switching language changed the URL parameter:
  - `page=...`
- This suggested potential file inclusion behavior

---

## Vulnerability Identified
- A Local File Inclusion (LFI) vulnerability was identified through the `page` parameter.
- The application dynamically included files based on user-controlled input, allowing directory traversal.
- This allowed access to sensitive Windows system files, confirming that arbitrary local file inclusion was possible.

---

## NTLM Hash Capture (Responder)
- it was possible to force the server to request a remote SMB resource.
- When a Windows system accesses an SMB path, it attempts NTLM authentication automatically.
- By pointing the inclusion request to an attacker-controlled SMB server, the target system leaked a NetNTLMv2 authentication challenge-response.
- Responder was used to capture the NetNTLMv2 hash of the user running the web service.

---

## Credential Recovery
- The captured NetNTLMv2 hash was cracked offline using a password wordlist attack.
- This revealed valid credentials for the Administrator account.

---

## Initial Access
- The machine exposed WinRM on port 5985
- The recovered Administrator credentials were reused to authenticate via WinRM
- This provided an interactive remote PowerShell session

---

## Privilege Escalation
- The compromised account was already a local Administrator

---

## Key Takeaways
- File inclusion vulnerabilities can be escalated beyond file disclosure on Windows targets
- LFI combined with SMB paths can trigger NTLM authentication leaks
- Responder is effective for capturing NetNTLMv2 challenge-response hashes
- Weak passwords allow NetNTLMv2 hashes to be cracked offline
- Exposed WinRM services become critical when administrator credentials are recovered
