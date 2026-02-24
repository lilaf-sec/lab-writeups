# Log Poisoning

---

## 📌 Definition
> **Log Poisoning** is a technique where an attacker injects malicious input into application or server logs, with the goal of having the log file interpreted or executed by the system (often through a Local File Inclusion vulnerability).

---

## 🛠️ Impact
- Remote Code Execution when log files are included and parsed by the application
- Privilege escalation through execution of injected payloads
- Persistent backdoor if logs are not rotated or sanitized
- Abuse of server-side logging mechanisms (e.g., Apache, Nginx, SSH logs)

---

## 🧪 Machines / Writeups

### 🟡 Medium
- [Poison](../Machines/Medium/Poison.md)
