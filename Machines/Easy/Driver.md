# HTB — Driver

**Official HTB page**: [Driver](https://app.hackthebox.com/machines/Driver?tab=machine_info)  

**Difficulty**: Easy  
**OS**: Windows  
**Release date**: 02 Oct, 2021  
**Platforms**: HackTheBox  
**Tags**: NTLM

---

## Initial Enumeration
- Nmap scan revealed open ports:  
  - 80 (HTTP – IIS 10.0)  
  - 135 (RPC)  
  - 445 (SMB)  
  - 5985 (WinRM)  
- SMB signing not required  
- Web application required authentication  

---

## Vulnerability Identified
- Default credentials `admin:admin` allowed access to the web panel  
- Firmware upload functionality available  
- Uploaded files were reviewed through an internal file share  

---

## Initial Access
- Uploaded a malicious `.scf` file  
- Captured NTLM authentication using **Responder**  
- Cracked the NTLM hash to recover valid credentials  
- Connected to the target via **WinRM**  
- Gained a shell as a low-privileged user  

---

## Privilege Escalation
- The **Print Spooler** service was enabled  
- Abused a Print Spooler privilege escalation technique  
- Achieved SYSTEM privileges  

---

## Key Takeaways
- File upload features that interact with SMB shares can expose NTLM hashes  
- SCF files are effective for triggering outbound authentication  
- Captured hashes often lead to credential reuse  
- Misconfigured Print Spooler service remains a common Windows privilege escalation vector  
