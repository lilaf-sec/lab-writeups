# HTB — Toolbox

**Official HTB page**: [Toolbox](https://app.hackthebox.com/machines/Toolbox?tab=machine_info)

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 12 Mar, 2021  
**Platforms**: HackTheBox  
**Tags**: FTP, Docker, SQL Injection, 

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 21 (FTP),  22 (SSH), 135 (MSRPC), 139 (NetBIOS), 443 (HTTPS), 445 (SMB), 5985 (WinRM)
- HTTPS website hosted a web application related to **MegaLogistics**
- SSL certificate revealed a hostname:
  - `admin.megalogistic.com`

---

## Discovery (FTP)
- Anonymous FTP access was available
- FTP contained a **Docker Toolbox** 
- Research revealed Docker Toolbox commonly uses default credentials:
  - `docker : tcuser`

---

## Vulnerability Identified
- The web application was vulnerable to **PostgreSQL SQL injection**
- Injection allowed database-level command execution
- This could be escalated to full remote code execution via PostgreSQL features

---

## Initial Access
- SQL injection was used to execute system commands through PostgreSQL
- Remote code execution was achieved by abusing PostgreSQL command execution methods
- This provided a foothold inside the environment

---

## Pivot / Internal Network Access
- After gaining access, internal enumeration revealed a Docker network
- Internal port scanning showed an SSH service exposed internally
- Using the Docker Toolbox default credentials, SSH access was obtained
- This provided access to a Windows Docker environment

---

## Privilege Escalation
- Further enumeration revealed an administrator directory containing SSH keys
- Extracting the administrator SSH private key allowed privilege escalation
- This led to full administrative access

---

## Key Takeaways
- Anonymous FTP can leak sensitive deployment tooling (Docker Toolbox)
- Default credentials remain a common and critical misconfiguration
- PostgreSQL SQL injection can escalate to full RCE through command execution features
- Internal service discovery is essential after initial compromise
- Docker environments often expose additional pivot paths
