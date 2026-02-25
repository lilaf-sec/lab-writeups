# HTB — Apocalyst

**Official HTB page**: [Apocalyst](https://app.hackthebox.com/machines/Apocalyst?tab=machine_info)

**Difficulty**: Medium  
**OS**: Linux  
**Release date**: 18 Aug, 2017  
**Platforms**: HackTheBox  
**Tags**: CMS, Steganography

---

## Initial Enumeration
- Nmap scan revealed open ports:
  - 22 (SSH)
  - 80 (HTTP)
- Web server hosted a **WordPress** instance
- Website title indicated a themed blog: *Apocalypse Preparation Blog*

---

## Content Discovery
- Generated a custom wordlist from the website using `cewl`
- Used the generated wordlist for directory fuzzing
- Discovered a hidden directory:
  - `/Rightiousness`

---

## Steganography Extraction
- The `/Rightiousness` page contained an image
- Extracted hidden content from the image using steganography tools
- Retrieved a `list.txt` file containing a wordlist

---

## Credential Discovery (WordPress)
- Identified a valid WordPress username:
  - `falaraki`
- Performed brute force using the extracted `list.txt`
- Successfully obtained valid WordPress credentials

---

## Initial Access
- Logged into the WordPress admin panel
- Modified a theme template to inject PHP code execution
- Achieved remote code execution through the modified theme file
- Gained a shell on the system through the web server context

---

## Privilege Escalation
- Found MySQL credentials in `wp-config.php` but they were not useful for escalation
- Enumerated writable files and discovered `/etc/passwd` was writable
- Abused the writable `/etc/passwd` to add a new privileged user entry
- Generated a password hash and inserted it into `/etc/passwd`
- Escalated privileges by switching to the newly created user

---

## Key Takeaways
- Wordlist generation from website content can improve fuzzing results significantly
- Steganography is a common method to hide wordlists and credentials
- WordPress credential brute forcing remains effective when weak passwords are used
- Theme template modification is a common path to WordPress RCE
- Writable system files such as `/etc/passwd` lead to immediate privilege escalation
- Misconfigured permissions can completely bypass traditional privilege escalation methods

