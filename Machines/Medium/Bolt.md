# HTB — Bolt

**Official HTB page**: [Bolt](https://app.hackthebox.com/machines/Bolt?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 25 Sep, 2021  
**Platforms**: HackTheBox  
**Tags**: Docker, SSTI

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP)
  - 443 (HTTPS)
- HTTPS website was running **Passbolt** (`passbolt.bolt.htb`)
- The HTTP website contained a **downloadable `.tar` archive**
- Subdomain enumeration revealed:
  - `demo.bolt.htb`
  - `mail.bolt.htb`

---

## Vulnerability Identified
- Publicly accessible archive contained Docker layers
- Docker image contained sensitive application files
- Application required an **invite code** for registration
- Web application vulnerable to **SSTI (Server-Side Template Injection)**
- Sensitive secrets stored encrypted inside a database
- User stored a leaked private key in Chrome extension data

---

## Initial Access
- Downloaded the `.tar` file and extracted Docker layers
- Found application source code inside the extracted container filesystem
- Discovered an `invite_code` inside `routes.py`
- Used the invite code to register on `demo.bolt.htb`
- Account was then usable on `mail.bolt.htb` (credential reuse)

---

## Exploitation (SSTI)
- Profile name update triggered an email confirmation message
- Input testing confirmed template evaluation:
  - `{{7*7}}` returned `49`
- Confirmed SSTI vulnerability (Jinja2)
- Achieved RCE through template payload execution
- Reverse shell obtained using an OS command injection payload

---

## Lateral Movement
- Enumeration with linpeas revealed credentials inside:
  - `/etc/passbolt/passbolt.php`
- MySQL credentials allowed access to the Passbolt database
- A `secrets` table contained an encrypted message
- The database password was reused as system credentials
- SSH access obtained as user `eddie`

---

## Privilege Escalation
- Further enumeration as `eddie` revealed Chrome extension data containing a private key:
  - `/home/eddie/.config/google-chrome/Default/Local Extension Settings/.../000003.log`
- Extracted and cleaned the private key format
- Used `gpg2john` + `john` to crack the GPG passphrase
- Imported the recovered key and decrypted the encrypted secret
- Decrypted message revealed the **root password**
- Switched to root using credential reuse

---

## Key Takeaways
- Public archives can leak full Docker images and sensitive source code
- Application source code disclosure often leads to authentication bypass (invite codes, API keys)
- SSTI can quickly escalate to full RCE
- Config files frequently contain reusable credentials
- Browser extension storage can leak sensitive secrets (private keys)
- Cracking GPG keys can lead to decrypting stored secrets
- Credential reuse remains one of the most effective escalation techniques

