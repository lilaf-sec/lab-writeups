# HTB — Nunchucks

**Official HTB page**: [Nunchucks](https://app.hackthebox.com/machines/Nunchucks?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 02 Nov, 2021  
**Platforms**: HackTheBox  
**Tags**: SSTI

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – redirect to HTTPS)  
  - 443 (HTTPS – nginx)  
- SSL certificate revealed domain:  
  - `nunchucks.htb`  
- Subdomain enumeration discovered:  
  - `store.nunchucks.htb`  

---

## Vulnerability Identified
- The `store` subdomain was vulnerable to **Server-Side Template Injection (SSTI)**  
- The backend used **Node.js** templating  
- Payload allowed execution of arbitrary system commands  

---

## Initial Access
- Exploited SSTI to execute commands via `child_process`  
- Retrieved reverse shell  
- Gained access as user `david`  

---

## Privilege Escalation
- `getcap` revealed:  
  - `/usr/bin/perl = cap_setuid+ep`  
- Perl allowed privilege escalation to UID 0  
- However, direct access to `/root` was blocked  

- AppArmor profile restricted access to sensitive paths  
- Identified AppArmor rule allowing execution of `/opt/backup.pl`  

- AppArmor shebang handling vulnerability allowed bypass  
- Created a custom Perl script with a shebang  
- Executed script to read `/root/root.txt`  

---

## Key Takeaways
- Subdomain enumeration often reveals hidden attack surfaces  
- SSTI in Node.js applications can directly lead to RCE  
- Linux capabilities can introduce unexpected privilege escalation paths  
- AppArmor restrictions can sometimes be bypassed through interpreter behavior  
- Misconfigured security policies may still allow full compromise  
