# HTB — NodeBlog

**Official HTB page**: [NodeBlog](https://app.hackthebox.com/machines/NodeBlog?tab=machine_info)

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 10 Jan, 2022  
**Platforms**: HackTheBox  
**Tags**: NoSQL Injection, XXE, Docker, Deserialization

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 5000 (HTTP – Node.js Express application)
- Web application running a custom blog on port 5000

---

## Vulnerability Identified (Authentication Bypass)
- `/login` endpoint accepted JSON input
- Application was vulnerable to **NoSQL injection**
- Authentication bypass achieved using MongoDB operator manipulation
- Gained access to authenticated functionality

---

## File Upload & XXE
- After login, application allowed XML file upload
- Error message revealed required XML structure
- Application parsed XML input without proper validation
- Exploited **XXE (XML External Entity)** vulnerability
- Achieved arbitrary file read (e.g., `/etc/passwd`)
- Enumerated internal network information via `/proc` files
- Confirmed containerized environment

---

## Source Code Disclosure
- Error messages revealed application path:
  - `/opt/blog`
- Retrieved `server.js` via XXE
- Discovered usage of:
  - `node-serialize`
- Application unserialized cookie data directly

---

## Remote Code Execution
- Identified vulnerability:
  - **CVE-2017-5941 (node-serialize RCE)**
- Cookie-based deserialization allowed execution of arbitrary JavaScript
- Crafted malicious serialized payload
- Achieved remote code execution
- Obtained reverse shell as application user

---

## Privilege Escalation
- Internal enumeration revealed MongoDB running locally (port 27017)
- Accessed local MongoDB instance without authentication
- Enumerated `blog` database
- Retrieved stored user credentials from `users` collection
- Reused credentials for privilege escalation
- Escalated to root via credential reuse

---

## Key Takeaways
- NoSQL injection can fully bypass authentication logic
- XML upload functionality frequently leads to XXE and file disclosure
- Source code disclosure dramatically increases attack surface
- Deserialization vulnerabilities can directly lead to RCE
- Internal databases often run without authentication
- Credential reuse remains a highly effective escalation technique

