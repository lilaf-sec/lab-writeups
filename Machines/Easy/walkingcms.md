# DockerLabs — walkingcms

**Official page**: [DockerLabs](https://dockerlabs.es/)

**Difficulty**: Easy  
**OS**: Linux  
**Platforms**: DockerLabs  
**Tags**: CMS

---

## Initial Enumeration
- Nmap scan revealed:
  - 80 (HTTP – Apache 2.4.57)
- Web server displayed default Apache page
- Directory fuzzing revealed:
  - `/wordpress`

---

## Vulnerability Identified
- WordPress instance exposed
- WPScan identified valid user:
  - `mario`
- Password brute force against WordPress login succeeded

---

## Initial Access
- Logged into WordPress admin panel
- Modified theme file:
  - `twentytwentytwo/index.php`
- Inserted PHP reverse shell payload
- Triggered payload through browser
- Received shell as `www-data`

---

## Privilege Escalation
- Enumerated SUID binaries
- Found:
  - `/usr/bin/env`
- Abused SUID env:
  - `env /bin/sh -p`
- Spawned root shell

---

## Key Takeaways
- WordPress brute force remains effective when weak credentials exist
- Theme editor provides direct code execution
- SUID env leads to immediate privilege escalation
- Basic SUID enumeration quickly reveals low-effort root paths
