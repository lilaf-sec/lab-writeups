# HTB — Jerry

**Official HTB page**: https://app.hackthebox.com/machines/Jerry?tab=machine_info

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 30 Jun, 2018  
**Platforms**: HackTheBox  
**Tags**: File upload

---

## Initial Enumeration
- Nmap scan revealed open port:
  - 8080 (HTTP – Apache Tomcat 7.0.88)
- Default Tomcat landing page accessible
- Tomcat Manager interface exposed

---

## Vulnerability Identified
- Tomcat Manager accessible with **default credentials**
- Authentication allowed deployment of applications
- `.war` file upload permitted via Manager interface
- Uploaded applications were executed by the server

---

## Initial Access
- Generated malicious WAR payload
- Uploaded WAR file through Tomcat Manager
- Obtained reverse shell
- Gained SYSTEM-level access directly

---

## Key Takeaways
- Default credentials remain a critical misconfiguration
- WAR deployment allows arbitrary JSP execution
- Always check for default credentials on administrative panels

