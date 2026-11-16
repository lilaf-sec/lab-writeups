# Deserialization

---

## 📌 Définition
> **Insecure Deserialization** is a vulnerability where an application deserializes untrusted data without proper validation, allowing an attacker to manipulate serialized objects and potentially alter application behavior.

---

## 🛠️ Impact
- Remote Code Execution (RCE) (depending on the language and gadget chains available)
- Authentication bypass (session or token manipulation)
- Privilege escalation by modifying object properties
- Sensitive data exposure through tampered serialized objects

---

## 🧪 Machines / Writeups

### 🟢 Easy
- [Broker](/Machines/Easy/Broker.md)
- [NodeBlog](/Machines/Easy/NodeBlog.md)
