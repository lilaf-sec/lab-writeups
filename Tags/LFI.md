# LFI - Local File Inclusion

---

## 📌 Définition
> **LFI (Local File Inclusion)** is a vulnerability where an attacker can trick an application into including local files from the server, usually by manipulating a parameter that controls file paths.

---

## 🛠️ Impact
- Reading sensitive files (e.g. `/etc/passwd`, configuration files, source code)
- Disclosure of credentials (database passwords, API keys)
- Log poisoning leading to Remote Code Execution (in some cases)
- Bypassing access controls by including restricted files

---

## 🧪 Machines / Writeups

[FriendZone (Easy)](../Machines/Easy/FriendZone.md)

