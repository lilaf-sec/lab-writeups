# Python

---

## 📌 Définition
> **Python vulnerabilities and misconfigurations** can be exploited when scripts are executed with elevated privileges, or when Python imports modules from insecure locations. Attackers can abuse this to execute arbitrary code or escalate privileges.

---

## 🛠️ Impact
- Privilege escalation via Python module/library hijacking
- Command execution through insecure Python scripts run as root (sudo / cron jobs)
- Abusing writable directories in Python import paths
- Exploitation of insecure dependencies or third-party libraries

---

## 🧪 Machines / Writeups

- [FriendZone (Easy)](../Machines/Easy/FriendZone.md)
- [Chaos (Medium)](../Machines/Medium/Chaos.md)
