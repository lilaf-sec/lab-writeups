# DNS - Domain Name System

---

## 📌 Définition
> **DNS (Domain Name System)** is a protocol used to resolve domain names into IP addresses. Misconfigurations in DNS servers can expose internal infrastructure and sensitive information, such as subdomains, internal hostnames, or network services.

---

## 🛠️ Impact
- Discovery of internal subdomains and services (information disclosure)
- DNS zone transfer (AXFR) leaking the full DNS configuration
- Easier attack surface mapping and target enumeration
- Potential access to hidden applications hosted on internal virtual hosts

---

## 🧪 Machines / Writeups

### 🟢 Easy
- [FriendZone](/Machines/Easy/FriendZone.md)
