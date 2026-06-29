# HTB — Arctic

**Official HTB page**: https://app.hackthebox.com/machines/Arctic?tab=machine_info

**Difficulty**: Easy  
**OS**: Windows  
**Platform**: HackTheBox  

**Tags**: FileUpload

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 135 (MSRPC)
  - 8500 (HTTP - ColdFusion / JRun)
  - 49154 (MSRPC)
- Web service on port 8500 exposed:
  - `/CFIDE/`
  - `/cfdocs/`
- ColdFusion 8 identified on the target

---

## Vulnerability Identification
- ColdFusion 8 exposed administration interface:
  - `/CFIDE/administrator/`
- Research revealed path traversal vulnerability
- Exploitation allows reading sensitive configuration files

---

## Credential Discovery
- Path traversal used to access:
  - `password.properties`
- Extracted hash found in configuration file
- Hash was cracked offline
- Retrieved valid ColdFusion administrator credentials

---

## Initial Access (ColdFusion Exploitation)
- Logged into ColdFusion administrator panel
- Identified available features:
  - Scheduled Tasks
  - Mappings
- Abuse of Scheduled Tasks allowed remote file execution
- Target supports JSP execution via ColdFusion engine

---

## Payload Delivery
- Generated JSP reverse shell payload:
  - `java/jsp_shell_reverse_tcp`
- Hosted payload on local HTTP server
- Created scheduled task pointing to remote JSP file
- Task executed and saved output to web directory

---

## Shell Access
- Reverse shell obtained after task execution
- Listener caught incoming connection
- Gained initial Windows shell access

---

## Privilege Escalation
- User had `SeImpersonatePrivilege` enabled
- This privilege allows token impersonation attacks
- Used Juicy Potato technique for privilege escalation

---

## Exploitation (Juicy Potato)
- Transferred required binaries to target:
  - exploit binary
  - reverse shell executable
- Executed Juicy Potato with appropriate COM object
- Spawned SYSTEM-level process successfully

---

## Root Access
- SYSTEM shell obtained
- Full administrative control achieved

---

## Key Takeaways
- ColdFusion 8 is vulnerable to multiple legacy attacks
- Path traversal can expose sensitive configuration files
- Scheduled tasks can be abused for remote code execution
- `SeImpersonatePrivilege` is a critical escalation vector on Windows
- Juicy Potato remains effective on older Windows builds
