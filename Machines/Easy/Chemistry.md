# HTB — Chemistry

**Official HTB page**: [Chemistry](https://app.hackthebox.com/machines/Chemistry?tab=machine_info)

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 19 Oct, 2024  
**Platforms**: HackTheBox  
**Tags**: File Upload, Python

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 5000 (HTTP – Werkzeug / Python 3.9.5)
- Web application named **Chemistry**
- Application allowed file uploads with `.cif` extension

---

## Vulnerability Identified (File Upload → RCE)
- Application processed uploaded `.cif` files using **Pymatgen**
- Pymatgen CIF parser vulnerable to arbitrary code execution
- Crafted malicious CIF file
- Achieved remote code execution through file upload
- Gained shell access as user `app`

---

## Pivot (Database Extraction)
- Located application source code (`app.py`)
- Discovered SQLite database configuration:
  - `database.db`
- Found database inside:
  - `instance/database.db`
- Extracted user table containing MD5 password hashes
- Cracked multiple hashes offline
- Identified valid credentials for user `rosa`
- Reused credentials to obtain SSH access

---

## Privilege Escalation
- Local enumeration revealed internal service running on:
  - `127.0.0.1:8080`
- Service identified as:
  - `Python/3.9 aiohttp/3.9.1`
- Version vulnerable to:
  - **CVE-2024-23334**
- Exploited aiohttp vulnerability
- Achieved elevated privileges
- Obtained root access

---

## Key Takeaways
- File upload validation must include content inspection, not only extension checks
- Pymatgen CIF parser can lead to direct RCE if unpatched
- SQLite databases often store weakly hashed credentials
- MD5 remains trivial to crack in CTF environments
- Internal-only services frequently expose privilege escalation paths
- Always enumerate localhost-bound services after initial compromise

