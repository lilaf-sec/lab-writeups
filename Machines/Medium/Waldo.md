# HTB — Waldo

**Official HTB page**: [Waldo](https://app.hackthebox.com/machines/Waldo?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 04 Aug, 2018  
**Platforms**: HackTheBox  
**Tags**: LFI  

---

## Initial Enumeration
- Nmap scan revealed:
  - 22 (SSH – OpenSSH 7.5)
  - 80 (HTTP – nginx 1.12.2)
- Web application exposed a **List Manager**
- Intercepted request while interacting with lists:
  - `POST /fileRead.php` → `file=./.list/list1`
- Forwarding the request triggered:
  - `POST /dirRead.php` → `path=./.list/`

---

## Vulnerability Identified
- Directory traversal in `path` parameter:
  - Allowed directory listing outside web root
- LFI in `file` parameter:
  - Allowed reading arbitrary files
- Enumerated filesystem using traversal:
  - Located `/home/nobody/.ssh/`
- Identified private key file:
  - `.monitor`

---

## Initial Access
- Retrieved private key via LFI
- Logged in as `nobody`
- Found `authorized_keys` referencing user `monitor`
- SSH access to `monitor` was possible locally
- Restricted shell (`rbash`) bypassed with:
  - `ssh -i .monitor monitor@localhost bash`

---

## Privilege Escalation
- Enumerated capabilities:
  - `/usr/bin/tac = cap_dac_read_search+ei`
  - `/home/monitor/app-dev/v0.1/logMonitor-0.1 = cap_dac_read_search+ei`
- Capability allowed reading files regardless of permissions
- Used:
  - `tac /root/root.txt`
- Retrieved root flag

---

## Key Takeaways
- Chained directory traversal + LFI leads to credential disclosure
- SSH keys exposed through LFI provide reliable access
- Restricted shells are often bypassable
- Linux capabilities can replace classic SUID for privilege escalation
- `cap_dac_read_search` allows reading any file on the system
