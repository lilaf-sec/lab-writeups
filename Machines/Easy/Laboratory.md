# HTB — Laboratory

**Official HTB page**: [Laboratory](https://app.hackthebox.com/machines/Laboratory?tab=play_machine)

**Difficulty**: easy  
**OS**: Linux  
**Release date**: 14 Nov, 2020  
**Platforms**: HackTheBox  
**Tags**: Path Hijacking

---

## Initial Enumeration
- Nmap revealed:
  - 22 (SSH)
  - 80 / 443 (Apache, redirect to `laboratory.htb`)
- The TLS certificate exposed a second virtual host: **git.laboratory.htb**
- That host served a **GitLab Community Edition 12.8.1** instance

## Vulnerability Identified
- GitLab 12.8.1 was vulnerable to **CVE-2020-10977**, an arbitrary file read through **path traversal** in the issue "move" feature
- The leaked server secret could be chained into **remote code execution** as the `git` user

## Initial Access
1. Registered an account and used the path-traversal issue (referenced in the public HackerOne report) to read sensitive server files
2. Chained the leak into **RCE** and obtained a shell inside the GitLab container
3. Opened a `gitlab-rails` production console and promoted a controlled account to **admin**:
   - `User.find_by(...)` → set `admin = true` → `save!`
4. With admin access, browsed a private project that stored a user's **SSH private key**
5. Logged in over SSH as the **dexter** user

## Privilege Escalation
- Found a **SUID** root binary `docker-security`
- `ltrace` showed it called `chmod` via **`system()` without an absolute path**
- Prepended a writable directory to `PATH` (`export PATH=/tmp:$PATH`) and placed a malicious `chmod` there
- Running `docker-security` executed the planted binary as **root**

## Key Takeaways
- Path-traversal file reads in GitLab can be chained to full RCE
- A `gitlab-rails` console grants complete control over the application, including admin promotion
- Secrets like SSH keys stored in private repos are still reachable once you are admin
- SUID binaries that call commands by relative path are vulnerable to PATH hijacking
- Always invoke external commands with absolute paths in privileged programs
