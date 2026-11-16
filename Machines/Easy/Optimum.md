# HTB — Optimum

**Official HTB page**: [Optimum](https://app.hackthebox.com/machines/Optimum?tab=play_machine)

**Difficulty**: easy  
**OS**: Windows  
**Release date**: 18 Mar, 2017  
**Platforms**: HackTheBox  
**Tags**: [Other](/Tags/Other.md)

---

## Initial Enumeration
- Nmap revealed a single service:
  - 80 (HTTP – **Rejetto HTTP File Server (HFS) 2.3**)
- The version banner alone was enough to identify a known vulnerability

## Vulnerability Identified
- HFS 2.3.x was vulnerable to **CVE-2014-6287**, a **remote command execution** flaw via a crafted `search` parameter (null-byte macro injection)
- The underlying Windows build was unpatched against several kernel privilege-escalation CVEs

## Initial Access
1. Identified the public exploit for HFS 2.3 (`searchsploit HFS 2.3`)
2. Used the RCE to pull a reverse-shell binary from an attacker-hosted HTTP server and execute it
3. Obtained a shell as the low-privileged web user

## Privilege Escalation
- Enumerated the OS build and missing patches with a Windows exploit-suggester
- The host was vulnerable to **MS16-032** (Secondary Logon Handle privilege escalation)
- Ran the public MS16-032 exploit → **SYSTEM**

## Key Takeaways
- A service version banner is often enough to find a reliable public exploit
- Rejetto HFS 2.3 has a well-known unauthenticated RCE (CVE-2014-6287)
- Unpatched Windows hosts frequently fall to kernel-level privilege-escalation CVEs
- Exploit-suggester tools quickly map a build number to missing patches
- Keep internet-facing software patched — old file servers are easy targets
