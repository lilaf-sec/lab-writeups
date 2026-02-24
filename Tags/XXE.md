# XXE - XML External Entity Injection

---

## 📌 Définition
> **XXE (XML External Entity Injection)** is a vulnerability that occurs when an application processes XML input with external entities enabled, allowing an attacker to define malicious entities to access internal files or services.

---

## 🛠️ Impact
- Reading sensitive local files (e.g. `/etc/passwd`, application configs)
- Disclosure of credentials or secret keys stored on the server
- SSRF by forcing the server to make internal network requests
- Denial of Service (DoS) through XML entity expansion (Billion Laughs attack)

---

## 🧪 Machines / Writeups

### 🟢 Easy
- [NodeBlog](../Machines/Easy/NodeBlog.md)

