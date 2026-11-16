# HTB — Heist

**Official HTB page**: [Heist](https://app.hackthebox.com/machines/Heist?tab=play_machine)

**Difficulty**: easy  
**OS**: Windows  
**Release date**: 10 Aug, 2019  
**Platforms**: HackTheBox  
**Tags**: Other

---

## Initial Enumeration
- Nmap revealed a Windows host with:
  - 80 (HTTP – IIS, "Support Login Page")
  - 135 / 445 (RPC / SMB)
  - 5985 (WinRM)
- The login page allowed authentication as **guest**
- An "issues" thread exposed a user, **Hazard**, asking support for help and sharing a **Cisco IOS configuration** file

## Vulnerability Identified
- The Cisco config contained a **type 7** password and a **type 5** secret, both reversible or crackable offline
- Credentials were **reused** across accounts, enabling password spraying
- A domain user left an **authenticated browser session** open, with credentials recoverable from process memory

## Initial Access
1. Decoded the Cisco **type 7** passwords and cracked the **type 5** secret offline
2. Built a user list (`Hazard`, `rout3r`, `admin`, …) and a password list from the recovered secrets
3. Sprayed with `netexec smb --continue-on-success` → valid login for **Hazard**
4. Enumerated more users through `--rid-brute` (revealing `Chase`, `Jason`, `support`)
5. Re-sprayed and found a working set of credentials for **Chase**, who was allowed over **WinRM** (`Pwn3d!`)

## Privilege Escalation
- Noticed several **Firefox** processes running under Chase's session
- Uploaded **ProcDump** and dumped the memory of the main Firefox process
- Downloaded the dump and ran `strings` → recovered an Administrator login submitted to `login.php`
- Logged in as **Administrator** over WinRM with the recovered password

## Key Takeaways
- Cisco type 7 passwords are trivially reversible and type 5 secrets are crackable — never share configs
- Password reuse turns a single leaked secret into domain-wide access
- `--rid-brute` is a quick way to expand a user list from one valid account
- Browser processes keep plaintext credentials in memory; a process dump can expose them
- Treat any file shared in a support channel as potentially sensitive
