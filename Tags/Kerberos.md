# Kerberos - Authentication Protocol

---

## 📌 Définition
> **Kerberos** is a network authentication protocol used in Active Directory environments to securely authenticate users and services using tickets instead of transmitting passwords directly. It relies on a Key Distribution Center (KDC), which includes the Authentication Service (AS) and Ticket Granting Service (TGS).

Kerberos is heavily used in Windows Active Directory domains and is a central component in domain authentication and privilege delegation.

---

## 🛠️ Impact
- Credential theft through ticket extraction (TGT / TGS)
- Offline password cracking via Kerberoasting
- Service account compromise due to weak SPN passwords
- Ticket reuse attacks (Pass-the-Ticket)
- Golden Ticket / Silver Ticket forging in compromised domains
- Lateral movement inside Active Directory environments

---

## 🧪 Machines / Writeups

### 🟢 Easy
- [Active](/Machines/Easy/Active.md)

