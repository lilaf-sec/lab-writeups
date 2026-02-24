# HTB — Irked

**Official HTB page**: [Irked](https://app.hackthebox.com/machines/Irked?tab=play_machine)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 17 Nov, 2018  
**Platforms**: HackTheBox  
**Tags**: Steganography

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – Apache)  
  - 111 (RPCBind)  
  - 6697 / 8067 / 65534 (IRC – UnrealIRCd)  
- IRC service identified as **UnrealIRCd**  

---

## Vulnerability Identified
- The IRC server was vulnerable to **CVE-2010-2075**  
- This backdoor allows remote command execution without authentication  

---

## Initial Access
- Connected to the IRC service  
- Triggered the backdoor command execution  
- Obtained a reverse shell on the target  

---

## Pivot
- Discovered a hidden file in the user’s home directory containing a password hint for **steganography**  
- Extracted embedded data from the website image using the discovered passphrase  
- Recovered user credentials and switched to a higher-privileged account  

---

## Privilege Escalation
- Found a SUID binary:  
  `/usr/bin/viewuser`  
- The binary attempted to execute a script located at:  
  `/tmp/listusers`  
- Created the missing script and abused it to execute commands as root  

---

## Key Takeaways
- Legacy services often contain publicly known backdoors  
- Steganography can hide credentials in plain sight  
- SUID binaries that execute external scripts are critical privilege escalation vectors  
