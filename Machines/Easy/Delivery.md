# HTB — Delivery

**Official HTB page**: [Delivery](https://app.hackthebox.com/machines/Delivery?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 09 Jan, 2021  
**Platforms**: HackTheBox  
**Tags**: Other

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 80 (HTTP – Nginx)  
  - 8065 (HTTP – Mattermost)  
- Web application on port 80 provided a ticketing system  

---

## Vulnerability Identified
- Created a support ticket using an email with the `@delivery.htb` domain  
- The ticketing system generated valid Mattermost account access  
- Internal chat logs exposed sensitive information:  
  - SSH credentials for user `maildeliverer`  
  - Password reuse policy based on variations of a common phrase  

---

## Initial Access
- Reused disclosed credentials to obtain SSH access as `maildeliverer`  

---

## Pivot
- Discovered Mattermost configuration file in `/opt/mattermost`  
- Extracted MySQL database credentials  
- Connected to the database and retrieved the administrator password hash  
- Generated wordlist variants based on the leaked password policy  
- Cracked the hash and recovered the administrator password  

---

## Privilege Escalation
- Reused the recovered credentials to switch to the `root` user  

---

## Key Takeaways
- Internal collaboration tools often leak critical operational secrets  
- Credential reuse remains a major security weakness  
- Configuration files frequently contain plaintext database credentials  
- Custom wordlists based on password policies greatly improve cracking success  
