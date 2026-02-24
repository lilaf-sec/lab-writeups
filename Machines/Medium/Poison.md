# HTB — Poison

**Official HTB page**: https://app.hackthebox.com/machines/Poison  

**Difficulty**: Medium  
**OS**: FreeBSD  
**Release date**: 24 Mar, 2018  
**Platforms**: HackTheBox  
**Tags**: LFI, Log Poisoning

---

## Initial Enumeration
- Nmap revealed ports 22 (SSH) and 80 (HTTP)
- Apache 2.4.29 running on FreeBSD with PHP 5.6
- The website allowed testing local PHP scripts via a `browse.php` parameter
- Several test scripts were listed (`ini.php`, `info.php`, `listfiles.php`, etc.)

---

## Vulnerability Identified — Local File Inclusion (LFI)
- The `file` parameter in `browse.php` was vulnerable to LFI
- `/etc/passwd` confirmed file disclosure
- A valid system user `charix` was identified

---

## Credential Discovery
- `listfiles.php` exposed a file named `pwdbackup.txt`
- The file contained a heavily Base64-encoded string
- After decoding multiple times, a plaintext password was recovered
- SSH access was obtained as user `charix`

---

## Alternative Exploitation — Log Poisoning to RCE
- The LFI allowed reading Apache access logs
- Malicious PHP code was injected via the `User-Agent` header
- Including the access log triggered code execution
- Remote command execution was achieved through the log inclusion

---

## Privilege Escalation
- A `secret.zip` file was found in the user’s home directory
- The archive password matched previously recovered credentials
- The extracted file was a VNC password file
- Process enumeration revealed a root-owned VNC server running 
- SSH local port forwarding exposed the VNC service
- Using the recovered VNC password granted root graphical access
- Root shell access was obtained from the VNC session

---

## Key Takeaways
- LFI vulnerabilities often lead to full system compromise
- Encoded backups frequently hide weak credential storage practices
- Log poisoning is a powerful technique when LFI is present
- Credential reuse significantly reduces exploitation complexity
- Misconfigured VNC services can expose privileged sessions
- Local port forwarding is essential for pivoting to internal services
