# HTB — Horizontall

**Official HTB page**: [Horizontall](https://app.hackthebox.com/machines/Horizontall?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 28 Aug, 2021  
**Platforms**: HackTheBox  
**Tags**: CMS, Api Abuse

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – Nginx)  
- Web server redirected to `horizontall.htb`  
-  Source Code Review
- JavaScript files referenced an additional subdomain:  
  - `api-prod.horizontall.htb`  
- The subdomain hosted a **Strapi CMS** instance  

---

## Vulnerability Identified

- Strapi CMS RCE
- Target running a vulnerable **Strapi 3.0.0-beta** version  
- Used public exploit to:  
  1. Obtain a valid JWT  
  2. Achieve authenticated RCE  

---

## Initial Access
- Triggered a reverse shell through the Strapi exploit  
- Gained foothold on the target system  

---

## Privilege Escalation

- Internal Service Discovery
- `ss -nltp` revealed multiple services bound to localhost  
- Used **Chisel** for reverse port forwarding to expose them locally  
-  Laravel Exploitation
- Discovered a Laravel v8 application on port 8000  
- Vulnerable to **CVE-2021-3129 (Laravel Ignition RCE)**  
- Executed the exploit to achieve code execution as **root**  

---

## Key Takeaways
- Client-side JavaScript is a valuable source for subdomain discovery  
- Outdated CMS platforms frequently lead to RCE  
- Port forwarding is essential to access internal-only services  
- Chaining multiple RCEs is a common real-world attack path  
- Laravel Ignition debug mode exposure leads to full system compromise  
