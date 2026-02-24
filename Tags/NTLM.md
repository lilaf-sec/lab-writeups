# NTLM - NT LAN Manager Hash Capture

---

## 📌 Définition
> **NTLM** is a suite of Microsoft security protocols used for authentication. Misconfigured services or vulnerable features (like SMB, SCF files, or web-based NTLM triggers) can leak challenge-response hashes, which can then be cracked offline to obtain valid credentials.

---

## 🛠️ Impact
- Capture of NetNTLMv1/v2 challenge-response hashes
- Offline cracking of hashes to recover valid user credentials
- Authentication bypass or privilege escalation using recovered credentials
- Access to services like SMB, WinRM, or web panels with compromised accounts

---

## 🧪 Machines / Writeups

### 🟢 Easy
- [Responder](../Machines/Easy/Responder.md)
- [Driver](../Machines/Easy/Driver.md)

### 🟡 Medium
- [Driver](../Machines/Medium/Jeeves.md)

