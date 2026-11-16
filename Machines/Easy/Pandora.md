# HTB — Pandora

**Official HTB page**: [Pandora](https://app.hackthebox.com/machines/Pandora?tab=play_machine)

**Difficulty**: easy  
**OS**: Linux  
**Release date**: 08 Jan, 2022  
**Platforms**: HackTheBox  
**Tags**: [SNMP](/Tags/SNMP.md), [SQL](/Tags/SQL.md), [Path Hijacking](/Tags/PathHijacking.md)

---

## Initial Enumeration
- TCP Nmap revealed only:
  - 22 (SSH)
  - 80 (Apache – "Play | Landing")
- The landing page was static; `whatweb` falsely hinted at WordPress, but the only `/wp-content/` reference pointed to an **external** domain, a dead end
- A **UDP** scan found **161 (SNMP)** open, running `net-snmp`

## Vulnerability Identified
- SNMP was reachable with the default **`public`** community string
- Process parameters exposed through SNMP leaked a user's **credentials in cleartext** (passed on a command line)
- An internal **Pandora FMS** instance (bound to localhost) was vulnerable to a known **SQL injection → RCE** chain

## Initial Access
1. Confirmed the community string with `onesixtyone`
2. Dumped the MIB tree with `snmpbulkwalk` and filtered `hrSWRunParameters`
3. Recovered SSH credentials for the user **daniel** from a process command line
4. Logged in over SSH as **daniel**

## Privilege Escalation
- An Apache vhost config revealed an internal Pandora FMS site running as the user **matt**, bound to localhost
- Set up an SSH **local port forward** to reach it
- Identified the version (`v7.0NG.742`) and used a public **Pandora FMS** exploit to get execution as **matt** (planted an SSH key for a stable shell)
- `matt` could run a SUID binary `/usr/bin/pandora_backup`
- `ltrace` showed it called `tar` **without an absolute path**
- Prepended a writable directory to `PATH` and placed a malicious `tar` → running `pandora_backup` executed it as **root**

## Key Takeaways
- Always scan UDP — services like SNMP are easy to miss on TCP-only scans
- Default community strings (`public`) expose the full process table, including secrets on command lines
- Never pass credentials as command-line arguments; they leak through SNMP and `ps`
- Internal-only services are still reachable through SSH port forwarding
- SUID binaries calling commands by relative path are vulnerable to PATH hijacking
