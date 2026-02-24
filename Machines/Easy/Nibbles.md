# HTB — Nibbles

**Official HTB page**: https://app.hackthebox.com/machines/Nibbles?tab=play_machine

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 30 Jun, 2018  
**Platforms**: HackTheBox  
**Tags**: File Upload, CMS

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP – Apache)
- Viewing page source revealed reference to:
  - `/nibbleblog/`
- Directory enumeration discovered:
  - `/nibbleblog/admin.php`

---

## Vulnerability Identified

- Credential Enumeration
- Username disclosed via:
  - `/nibbleblog/content/private/users.xml`
- Login system included rate limiting / blacklist
- Manual password guessing required
- Successfully authenticated to Nibbleblog admin panel

---

## Initial Access

- Nibbleblog File Upload
- Identified known exploit for:
  - Nibbleblog file upload vulnerability
- Uploaded malicious file via plugin functionality
- Achieved remote command execution
- Obtained shell access as user `nibbler`

---

## Privilege Escalation

- `sudo -l` revealed:
  - NOPASSWD entry for `/home/nibbler/personal/stuff/monitor.sh`
- Target script did not exist
- Created malicious script at expected path
- Executed script via sudo
- Escalated privileges to root

---

## Key Takeaways
- Page source often reveals hidden directories
- XML files may expose valid usernames
- Login rate limiting can be bypassed with careful manual attempts
- CMS plugin upload functionality frequently leads to RCE
- NOPASSWD entries pointing to non-existent files are dangerous
- Writable paths combined with sudo execution lead directly to root
