# HTB — Chaos

**Official HTB page**: [Chaos](https://app.hackthebox.com/machines/Chaos?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 15 Dec, 2018  
**Platforms**: HackTheBox  
**Tags**: LatexInjection, Python

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 80 (HTTP)
  - 110/995 (POP3/POP3S)
  - 143/993 (IMAP/IMAPS)
  - 10000 (Webmin)
- Accessing the web server by IP returned: **"Direct IP not allowed"**
  - Indicating **virtual hosting**
- Added `chaos.htb` to `/etc/hosts` to access the website
- Subdomain fuzzing revealed:
  - `wordpress.chaos.htb`
  - `webmail.chaos.htb`

---

## Vulnerability Identified
- WordPress blog post protected by weak password
- Credentials leaked inside a private WordPress post
- Webmail account contained encrypted files and an encryption script
- Encrypted message revealed a hidden internal service
- PDF generation web app vulnerable to **LaTeX injection**
  - Allowed file read (LFI)
  - Allowed command execution (RCE)
- Access provided a restricted shell (`rbash`)
- Firefox profile stored Webmin root credentials
- Credential reuse allowed root access via Webmin

---

## Initial Access
- Found WordPress user `human`
- Post was password protected but used weak password:
- Post contained credentials for `webmail`
- Logged into `webmail.chaos.htb` (also accessible via IMAP using Claws-Mail)
- Mailbox contained:
  - `enim_msg.txt` (encrypted message)
  - `en.py` (encryption script)

---

## Decryption and Hidden Service Discovery
- Using the encryption script, decrypted `enim_msg.txt` with password
- Decrypted output contained Base64 data
- Base64 decoding revealed a hidden service URL
- The hidden page provided a service that generated PDFs from user input
- Directory fuzzing revealed a `/pdf/` directory containing generated PDF files

---

## Exploitation (LaTeX Injection)
- Confirmed LaTeX injection by reading `/etc/passwd`
- LaTeX payload allowed file disclosure
- Command execution was possible using LaTeX injection
- This led to remote code execution on the server

---

## Lateral Movement
- Reused the WordPress password to access user `ayush`
- User shell was restricted (`rbash`)
- Escaped rbash using `tar` checkpoint execution
- Restored normal environment by resetting the PATH

---

## Privilege Escalation
- Found a `.mozilla` directory in the user home directory
- Extracted Firefox profile files
- Used `firefox_decrypt` to recover saved credentials
- Firefox stored credentials for root

---

## Key Takeaways
- Virtual hosting restrictions often reveal hidden hostnames and subdomains
- Weak WordPress post passwords can leak sensitive credentials
- Webmail services are valuable targets for internal information disclosure
- LaTeX injection can escalate from LFI to full RCE
- Restricted shells are often bypassable using allowed binaries (`tar`)
- Browser credential stores can expose privileged credentials
