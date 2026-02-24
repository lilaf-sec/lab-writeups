# HTB — Mirai

**Official HTB page**: [Mirai](https://app.hackthebox.com/machines/Mirai?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 01 Sep, 2017  
**Platforms**: HackTheBox  
**Tags**: Other

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 22 (SSH)  
  - 53 (DNS – dnsmasq)  
  - 80 (HTTP – lighttpd)  
  - 1765 / 32469 (UPnP)  
  - 32400 (Plex Media Server)  
- HTTP response contained the header:  
  - `X-Pi-hole: A black hole for Internet advertisements`  
- Target identified as a **Raspberry Pi running Pi-hole**

---

## Initial Access
- Raspberry Pi systems commonly use default credentials  
- SSH access obtained using:  
  - `pi : raspberry`  
- Default password had not been changed  

---

## Privilege Escalation
- The `pi` user had sudo privileges  
- Root access obtained  

---

## Post-Exploitation
- `root.txt` was not present in the filesystem  
- Discovered a mounted USB device:  
- The device was mounted as read-only  

---

## Root Flag Recovery
- The USB contained deleted files  
- Used `strings` on the block device to recover file contents  
- Retrieved the deleted `root.txt` from the raw device  

---

## Key Takeaways
- Default credentials remain a critical security risk  
- Embedded devices often expose multiple network services  
- Mounted removable media can contain sensitive data  
- Deleted files can still be recovered from raw devices  
