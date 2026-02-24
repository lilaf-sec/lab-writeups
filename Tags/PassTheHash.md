# Pass-the-Hash

---

## 📌 Définition
> **Pass-the-Hash (PtH)** is a technique where an attacker authenticates to a Windows system using a stolen NTLM hash instead of the plaintext password. This allows access without cracking the hash.

---

## 🛠️ Impact
- Authentication as another user without knowing the password
- Privilege escalation to Administrator if hashes are recovered
- Lateral movement across Windows systems in a network
- Full domain compromise in Active Directory environments (if high-level hashes are obtained)

---

## 🧪 Machines / Writeups

### 🟢 Easy
- [Responder](../Machines/Easy/Responder.md)

### 🟡 Medium
- [Jeeves](../Machines/Medium/Jeeves.md)
