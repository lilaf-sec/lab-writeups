# HTB — Antique

**Official HTB page**: [Antique](https://app.hackthebox.com/machines/Antique)

**Difficulty**: Easy  
**OS**: Linux  
**Release date**: 27 Sep, 2021  
**Platforms**: HackTheBox  
**Tags**: SNMP

---

## Initial Enumeration
- UDP scan revealed port 161 open
- SNMP service identified
- SNMP enumeration with the default community string `public` returned data
- An OID exposed a hexadecimal-encoded value
- Decoding the hex data revealed valid credentials

---

## Initial Access
- The recovered credentials allowed access to a Telnet service
- The service was identified as HP JetDirect
- The interface exposed an `exec` functionality
- Command execution was confirmed
- A reverse shell was obtained as user `lp`

---

## Privilege Escalation
- The compromised user was part of the `lpadmin` group
- The CUPS service was running locally
- The `cupsctl` utility allowed modification of logging configuration
- The error log path was redirected to `/root/root.txt`
- Accessing the CUPS error log revealed the root flag

---

## Key Takeaways
- SNMP can expose sensitive information when misconfigured
- Hex-encoded values in SNMP output may contain credentials
- Printer services like JetDirect can expose dangerous administrative functionality
- Group misconfigurations can allow abuse of privileged services
- CUPS log manipulation is a classic privilege escalation technique

