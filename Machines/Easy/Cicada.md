# HTB — Cicada

**Official HTB page**: https://app.hackthebox.com/machines/Cicada?tab=machine_info

**Difficulty**: Easy  
**OS**: Windows  
**Platform**: HackTheBox  

**Tags**: ActiveDirectory, SMB, PassTheHash

---

## Initial Enumeration
- Nmap scan revealed a Domain Controller exposing:
  - DNS
  - Kerberos
  - LDAP
  - SMB
  - WinRM
- Domain identified as:
  - `cicada.htb`
- SMB signing was enabled
- Null authentication was allowed on SMB

---

## SMB Enumeration
- Connected to SMB as the `guest` account
- Enumerated available shares
- `HR` share was readable
- Welcome document disclosed the company's default password policy

---

## User Enumeration
- Kerberos user enumeration identified several valid domain users
- RID brute forcing over SMB revealed additional usernames
- AS-REP roasting was attempted but no users were vulnerable

---

## Credential Discovery
- Sprayed the default password against discovered users
- Obtained valid credentials for `michael.wrightson`
- WinRM access was not allowed with this account

---

## Lateral Movement
- Authenticated to SMB using `michael.wrightson`
- Enumerated domain information with authenticated RPC
- User description for `david.orelious` contained a plaintext password
- Authenticated successfully as `david.orelious`

---

## Additional Credential Discovery
- `david.orelious` had read access to the `DEV` share
- Found a PowerShell backup script
- Script contained hardcoded credentials for `emily.oscars`
- Credentials allowed successful WinRM authentication

---

## Initial Access
- Established a WinRM session as `emily.oscars`
- Obtained an interactive shell on the Domain Controller

---

## Privilege Escalation
- Enumerated user privileges
- `SeBackupPrivilege` was enabled
- Used the privilege to back up:
  - SAM hive
  - SYSTEM hive
- Extracted the registry hive files from the target

---

## Pass-the-Hash
- Recovered local Administrator NTLM hash from the registry hives
- Authenticated using Pass-the-Hash
- Obtained an Administrator WinRM session

---

## Root Access
- Full administrative access obtained through Pass-the-Hash

---

## Key Takeaways
- Guest SMB access can leak valuable information during enumeration
- Password spraying against default credentials remains effective
- User descriptions may expose sensitive credentials
- Hardcoded credentials inside administrative scripts are common attack vectors
- `SeBackupPrivilege` allows extraction of sensitive registry hives
- Pass-the-Hash remains an effective technique once NTLM hashes are recovered
