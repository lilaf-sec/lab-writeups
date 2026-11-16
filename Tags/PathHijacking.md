# Path Hijacking

---

## 📌 Définition
> **Path Hijacking** is a privilege-escalation technique where a privileged program calls an external command by name instead of by absolute path. By prepending a writable directory to `PATH` (often through a SUID binary or a `sudo` rule with `SETENV`), an attacker makes the program run a malicious binary of the same name.

---

## 🛠️ Impact
- Arbitrary command execution with the privileges of the vulnerable program (often root)
- Reliable local privilege escalation from a limited user
- Abuse of legitimate SUID binaries and sudo rules without exploiting a memory bug
- Full system compromise when the target process runs as root

## 🧪 Machines / Writeups

### 🟢 Easy
- [Laboratory](/Machines/Easy/Laboratory.md)
- [Pandora](/Machines/Easy/Pandora.md)
- [Photobomb](/Machines/Easy/Photobomb.md)
- [Previse](/Machines/Easy/Previse.md)
