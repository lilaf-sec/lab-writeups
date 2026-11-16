# HTB — Sauna

**Official HTB page**: [Sauna](https://app.hackthebox.com/machines/Sauna?tab=play_machine)

**Difficulty**: easy  
**OS**: Windows  
**Release date**: 15 Feb, 2020  
**Platforms**: HackTheBox  
**Tags**: Active Directory, Kerberos

---

## Initial Enumeration
- Nmap revealed a Windows Domain Controller: DNS (53), Kerberos (88), LDAP (389/3268), SMB (445), WinRM (5985) → domain `EGOTISTICAL-BANK.LOCAL`
- The "Egotistical Bank" website exposed an employee name: **Hugo Smith**
- An anonymous LDAP query returned the domain naming contexts, confirming the structure
- Built a list of candidate usernames from common naming conventions (`Fsmith`, `Hsmith`, `Hugo.Smith`, …)

## Vulnerability Identified
- The account `Fsmith` had **Kerberos pre-authentication disabled** → vulnerable to **AS-REP Roasting**
- The service account `svc_loanmgr` held **DCSync** rights over the domain → full domain compromise possible

## Initial Access
1. Validated usernames with `kerbrute userenum` → confirmed `Fsmith` and `Hsmith`
2. Performed **AS-REP Roasting** with `GetNPUsers.py` → retrieved an AS-REP hash for `Fsmith`
3. Cracked the hash offline with **John the Ripper**
4. Logged in as `FSmith` over **WinRM** using `evil-winrm`

## Privilege Escalation
- Enumerated local groups and identified the service account `svc_loanmgr`
- **WinPEAS** revealed stored auto-logon credentials for `svc_loanmgr`
- **BloodHound** showed `svc_loanmgr` held **DCSync** rights (GetChanges / GetChangesAll)
- Ran a **DCSync** attack with `secretsdump.py` to dump the Administrator NTLM hash
- Logged in as **Administrator** via **Pass-the-Hash** with `evil-winrm`

## Key Takeaways
- Accounts without Kerberos pre-authentication can be AS-REP roasted and cracked offline
- Stored auto-logon credentials are a common and dangerous privilege-escalation path
- DCSync rights on any account are equivalent to full domain compromise
- Service accounts should follow least privilege and never hold replication rights
- BloodHound quickly surfaces attack paths that manual enumeration can miss
