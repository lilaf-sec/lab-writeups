# HTB — Spectra

**Official HTB page**: [Spectra](https://app.hackthebox.com/machines/Spectra?tab=machine_info)

**Difficulty**: Easy  
**OS**: ChromeOS  
**Release date**: 27 Feb, 2021
**Platforms**: HackTheBox  
**Tags**: CMS, Python

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH),  80 (HTTP),  3306 (MySQL)
- Web server running **nginx**
- MySQL service exposed but not directly accessible
- Website hosted a WordPress instance

---

## Information Disclosure
- Discovered a backup configuration file:
  - `wp-config.php.save`
- The file contained database credentials
- Credentials allowed login as **WordPress administrator**

---

## Vulnerability Identified
- WordPress admin access allowed modification of site components
- Theme editor initially blocked direct modification
- Plugin editor allowed editing of plugin files
- Modified an existing plugin file to inject PHP command execution
- Achieved RCE via the modified plugin. The reverse shell only works with Python

---

## Initial Access
- Confirmed command execution via web request
- Reverse shell obtained using a Python-based payload
- Gained shell access as a low-privileged user

---

## Pivot / Enumeration
- Discovered an `autologin.conf.orig` file inside `/opt`
- File contained credentials for a local user
- User was part of the `developers` group

---

## Privilege Escalation
- Files inside `/etc/init/` were writable by the `developers` group
- Upstart configuration files could be modified
- User was allowed to start init jobs using `initctl`
- Modified one of the init configuration files to execute a privileged command
- Started the modified service
- Escalated privileges by abusing the init job execution context

---

## Key Takeaways
- Backup configuration files frequently leak critical credentials
- WordPress plugin editing can provide reliable RCE once admin access is obtained
- Group-based write permissions on init configuration files are highly dangerous
- Misconfigured service control mechanisms (initctl) can lead to direct root execution
- Always audit writable system configuration files for privilege escalation paths

