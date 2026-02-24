# HTB — Union

**Official HTB page**: [Union](https://app.hackthebox.com/machines/Union?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 22 Nov, 2021  
**Platforms**: HackTheBox  
**Tags**: SQL  

---

## Initial Enumeration
- Initial Nmap scan revealed:
  - 80 (HTTP – nginx 1.18.0)
- Directory enumeration revealed:
  - `/index.php`
  - `/challenge.php`
  - `/firewall.php`
  - `/config.php`
- Web application contained a challenge requiring a flag submission

---

## Vulnerability Identified
- SQL Injection identified in `player` parameter on main page
- Classic injections (`OR`, `AND`, `SLEEP`) did not work
- `UNION SELECT` injection successful
- Performed full SQL enumeration
- Discovered `flag` table with column containing valid flag
- Submitted flag through web interface
- Application granted SSH access to attacker IP

---

## Initial Access
- After flag submission, port 22 became accessible
- Used SQL injection with:
  - `UNION SELECT load_file("/var/www/html/config.php")`
- Extracted SSH credentials from configuration file
- Logged in via SSH

---

## Privilege Escalation
- Located `firewall.php` in `/var/www/html`
- Script executed:
  - `sudo /usr/sbin/iptables -A INPUT -s $ip -j ACCEPT`
- `$ip` value derived from `X-Forwarded-For` header
- No validation performed on header input
- Command injection possible via crafted header
- Achieved command execution as `www-data`
- `sudo -l` revealed:
  - `(ALL : ALL) NOPASSWD: ALL`
- Injected command to grant SUID to `/bin/bash`
- Executed `/bin/bash -p`
- Obtained root shell

---

## Key Takeaways
- UNION-based SQL injection can bypass basic filtering
- SQL `load_file()` allows sensitive file disclosure
- Trusting HTTP headers without validation leads to command injection
- Web services running sudo commands are high-risk
