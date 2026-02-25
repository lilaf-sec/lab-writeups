# HTB — Flustered

**Official HTB page**: [Flustered](https://app.hackthebox.com/machines/Flustered?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 31 Jan, 2022  
**Platforms**: HackTheBox  
**Tags**: Squid Proxy, SSTI 

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP – nginx)
  - 111 (RPC)
  - 3128 (Squid Proxy)
  - 24007 (GlusterFS)
  - 49152/49153 (RPC / SSL)
- Website displayed:
  - `steampunk-era.htb - Coming Soon`
- Squid proxy required authentication

---

## GlusterFS Enumeration
- Listed available Gluster volumes:
  - `vol1`
  - `vol2`
- `vol1` required client certificates
- Successfully mounted `vol2`
- Extracted MariaDB data files from mounted volume
- Performed string analysis on database files
- Discovered proxy credentials

---

## Proxy Pivot
- Used discovered credentials to authenticate to Squid proxy
- Accessed internal-only services via proxy
- Performed directory fuzzing through the proxy
- Discovered `/app` directory and `app.py`

---

## Vulnerability Identified (SSTI)
- `app.py` used `render_template_string`
- User-controlled JSON value directly embedded into template
- Confirmed **Server-Side Template Injection (SSTI)**
- Achieved remote command execution
- Obtained reverse shell on the host

---

## GlusterFS Certificate Abuse
- Discovered GlusterFS certificate files on compromised system:
  - `glusterfs.ca`
  - `glusterfs.key`
  - `glusterfs.pem`
- Transferred certificates to attacker machine
- Successfully mounted `vol1`
- Found home directory of user `jennifer`
- Added SSH public key to `authorized_keys`
- Obtained SSH access as `jennifer`

---

## Privilege Escalation

### Docker Network Discovery
- Identified Docker network:
  - `172.17.0.0/16`
- Discovered active container at:
  - `172.17.0.2`
- Identified open internal service:
  - Port 10000

### Internal Service Analysis
- Port forwarding exposed internal service locally
- Service identified as:
  - Azurite (Azure Storage emulator)

### Key Disclosure
- Located storage access key in:
  - `/var/backups/key`
- Used key to access Azurite storage
- Retrieved root SSH private key from storage
- Used key to obtain root access

---

## Key Takeaways
- GlusterFS volumes can expose full database backends
- Database file analysis can reveal service credentials
- Authenticated proxies often hide internal attack surfaces
- SSTI in Flask (`render_template_string`) leads directly to RCE
- Client certificate-based services can be abused post-compromise
- Cloud storage emulators may contain sensitive keys and secrets
- Backup directories often leak critical access material
