# HTB — Admirer

**Official HTB page**: https://app.hackthebox.com/machines/Admirer?tab=machine_info

**Difficulty**: Easy  
**OS**: Linux  
**Platform**: HackTheBox  

**Tags**: FTP, CMS, Python

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 21 (FTP)
  - 22 (SSH)
  - 80 (HTTP)
- HTTP service running on Apache
- `robots.txt` revealed a hidden directory:
  - `/admin-dir`

---

## Web Enumeration
- Directory fuzzing on `/admin-dir/` revealed:
  - `contacts.txt`
- `contacts.txt` contained internal emails and usernames
- Further enumeration with a custom wordlist revealed:
  - `credentials.txt`

---

## Credential Discovery
- `credentials.txt` exposed FTP credentials
- FTP access granted multiple files:
  - `html.tar.gz`
  - `dump.sql`
- Extracting archive revealed web root structure:
  - `index.php`
  - `robots.txt`
  - `utility-scripts/`
  - `w4ld0s_s3cr3t_d1r/`

---

## Sensitive File Analysis
- `w4ld0s_s3cr3t_d1r` contained:
  - `contacts.txt`
  - `credentials.txt`
- `db_admin.php` and `index.php` contained database-related credentials
- Credentials were reused in multiple places but access via Adminer initially failed

---

## Vulnerability Identification
- Research on Adminer revealed potential exploitation via:
  - `LOAD DATA LOCAL INFILE`
- This allows reading local files from the server through database interaction
- Misconfiguration enables file disclosure via database queries

---

## Exploitation (Adminer File Read)
- Local database was prepared for connection
- Remote connection established via Adminer interface
- Used `LOAD DATA LOCAL INFILE` to read server files
- Retrieved:
  - `/var/www/html/index.php`
- Extracted credentials for user `waldo`

---

## Initial Access
- SSH access obtained using recovered credentials
- User shell: `waldo`

---

## Privilege Escalation
- Checked sudo permissions:
  - Allowed execution of `/opt/scripts/admin_tasks.sh` with environment variables
- Script calls Python backup utility:
  - `/opt/scripts/backup.py`

---

## Python Library Hijacking
- `backup.py` imports `shutil`
- Environment variable `PYTHONPATH` is modifiable
- Created malicious local `shutil.py`
- Hijacked import chain via:
  - `PYTHONPATH` manipulation
- Executed privileged script with sudo:
  - Triggered code execution as root

---

## Root Access
- Privilege escalation achieved via Python module hijacking
- Root shell obtained through manipulated backup execution chain

---

## Key Takeaways
- FTP leaks often expose internal web structure
- Directory fuzzing is critical for hidden admin panels
- Adminer can be abused via `LOAD DATA LOCAL INFILE`
- Python PATH hijacking is a powerful privilege escalation vector
- Misconfigured sudo scripts can lead to full root compromise
