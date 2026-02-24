# HTB — Popcorn

**Official HTB page**: [Popcorn](https://app.hackthebox.com/machines/Popcorn?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 15 Mar, 2017  
**Platforms**: HackTheBox  
**Tags**: File upload  

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH – OpenSSH 5.1p1 Ubuntu)
  - 80 (HTTP – Apache 2.2.12)
- Port 80 redirected to `popcorn.htb`
- Directory enumeration revealed:
  - `/torrent`
- The `/torrent` path hosted a torrent management application

---

## Vulnerability Identified
- The torrent application allowed uploading `.torrent` files
- After uploading a torrent, it was possible to edit the associated image
- File upload validation relied on extension and Content-Type
- Uploaded a `.php` file disguised as an image
- Set `Content-Type: image/jpeg` to bypass filtering
- The uploaded file contained a PHP system execution payload
- Achieved remote command execution via web shell

---

## Initial Access
- Triggered command execution through the uploaded PHP file
- Obtained a reverse shell as the web server user

---

## Privilege Escalation
- Kernel version identified:
  - Linux 2.6.31-14-generic-pae
- The kernel was vulnerable to **Dirty COW**
- Exploited Dirty COW (PTRACE_POKEDATA race condition)
- Modified `/etc/passwd` to escalate privileges
- Gained full root access

---

## Key Takeaways
- File upload filters based only on extension and Content-Type are insecure
- Image upload functionalities are common RCE entry points
- Legacy Linux kernels are frequently vulnerable to Dirty COW
- Kernel version enumeration is critical during privilege escalation
