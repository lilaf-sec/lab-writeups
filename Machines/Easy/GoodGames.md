# HTB — GoodGames

**Official HTB page**: [GoodGames](https://app.hackthebox.com/machines/GoodGames?tab=machine_info)

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 21 Feb, 2022  
**Platforms**: HackTheBox  
**Tags**: SQL, SSTI, Docker

---

## Initial Enumeration
- Nmap scan revealed open port:
  - 80 (HTTP)
- Web application running on Werkzeug (Python)
- Virtual host discovered later:
  - `internal-administration.goodgames.htb`

---

## Vulnerability Identified
- Login form vulnerable to **SQL injection**
- SQL injection allowed:
  - authentication bypass
  - database enumeration
  - credential extraction
- Admin panel contained a **Server-Side Template Injection (SSTI)** vulnerability
- SSTI allowed **remote command execution**
- Reverse shell landed inside a **Docker container**
- Container had a writable mount to the host filesystem (`/home/augustus`)
- Writable host mount enabled privilege escalation to root

---

## Initial Access (SQL Injection)
- Login bypass achieved using a simple payload:
  - `or 1=1-- -`
- Successful authentication granted admin access to the main website
- Admin access revealed an internal portal:
  - `http://internal-administration.goodgames.htb/login`
- No credentials were available yet, requiring further SQL enumeration

---

## Database Enumeration and Credential Extraction
- Identified number of columns using `ORDER BY`
- Confirmed 4 columns were reflected in the response
- Used `UNION SELECT` to extract database information
- Enumerated schema and tables from `information_schema`
- Dumped the `user` table and extracted credentials:
  - `admin:<hash>`
- Hash identified as MD5
- Cracked MD5 hash using John the Ripper
- Successfully logged into:
  - `internal-administration.goodgames.htb`

---

## Exploitation (SSTI → RCE)
- Internal administration portal contained an SSTI vulnerability in settings
- Confirmed template injection through payload testing
- Achieved command execution using Jinja2 SSTI payload
- Reverse shell obtained by executing a bash callback command

---

## Lateral Movement / Pivot (Container to Host)
- Reverse shell was obtained as root inside a Docker container
- Container enumeration showed a mounted host directory:
  - `/home/augustus`
- Local port scanning from the container revealed the host was reachable:
  - SSH was accessible internally
- Reused the cracked password to SSH into the host as user `augustus`

---

## Privilege Escalation
- Since the container had root access and `/home/augustus` was writable:
  - Copied `/bin/bash` into `/home/augustus`
  - Changed ownership to root
  - Set the SUID bit
- Logged back in as `augustus` and executed:
  - `./bash -p`
- Gained full root shell on the host system

---

## Key Takeaways
- SQL injection remains one of the fastest ways to obtain admin access
- Credential reuse across services is extremely common
- SSTI can directly lead to RCE with minimal effort
- Docker containers are not a security boundary if host mounts are writable
- Writable mounted directories can allow privilege escalation via SUID binaries

