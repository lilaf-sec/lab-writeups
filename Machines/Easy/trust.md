# DockerLabs — trust

**Official page**: [DockerLabs](https://dockerlabs.es/)

**Difficulty**: Easy  
**OS**: Linux  
**Platforms**: DockerLabs  

---

## Initial Enumeration
- Nmap scan revealed:
  - 22 (SSH – OpenSSH 9.2p1)
  - 80 (HTTP – Apache 2.4.57)
- Web server displayed default Apache page
- Directory fuzzing revealed:
  - `/secret.php`
- `secret.php` exposed user:
  - `mario`

---

## Vulnerability Identified
- Valid username disclosed through web content
- SSH service accessible externally

---

## Initial Access
- Performed SSH brute force against user `mario`
- Retrieved valid credentials
- Logged in via SSH as `mario`

---

## Privilege Escalation
- `sudo -l` revealed:
  - `(ALL) /usr/bin/vim`
- Abused vim shell escape:
  - `:set shell=/bin/bash`
  - `:shell`
- Spawned root shell

---

## Key Takeaways
- Hidden web files often leak valid usernames
- Weak SSH passwords remain a common entry point
- Sudo access to vim provides immediate shell escape
- Simple GTFOBins techniques are essential during privesc
