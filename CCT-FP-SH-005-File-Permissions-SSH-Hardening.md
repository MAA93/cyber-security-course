# DAY 5 - FILE PERMISSIONS & SSH HARDENING | CCT-FP-SH-005

## LEARN (15 min)
File permissions and SSH access are critical for securing Linux systems. Misconfigured SUID/SGID files and weak SSH settings can allow attackers to escalate privileges.

**Key concepts:**  
- SUID/SGID files  
- SSH key authentication  
- Disabling password login for SSH (information only, do not apply)  

## RESOURCES
- Linux File Permissions – https://www.cyberciti.biz/faq/linux-unix-check-suid-sgid-files/  
- SSH Key Authentication – https://www.ssh.com/academy/ssh/key  
- Hardening SSH – https://www.ssh.com/academy/ssh/hardening  
- Windows: Using SSH keys (PuTTYgen) – https://www.youtube.com/watch?v=q9YA5H53IHQ  
- Unix/Linux: SSH Key Authentication – https://www.youtube.com/watch?v=5Fcf-8LPvws&t=36s  

## DO (30 min)

### 1. SSH into your lab server
```bash
ssh [your-username]@[your-server-ip]
```

### 2. Check SUID/SGID files
```bash
# Find SUID files
find / -type f -perm /4000 2>/dev/null > CCT-FP-SH-005-suid.txt

# Find SGID files
find / -type f -perm /2000 2>/dev/null > CCT-FP-SH-005-sgid.txt
```
Info: SUID/SGID files allow elevated privileges. Misconfigured files are security risks.
### 3. Configure SSH key authentication

**Linux/Unix:**
```bash
# Generate SSH key pair on your machine
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -C "student"

# Copy public key to lab server
ssh-copy-id [your-username]@[your-server-ip]
```
**Optional Linux/Unix video reference:** https://www.youtube.com/watch?v=5Fcf-8LPvws&t=36s


**Windows:**  
- Use PuTTYgen to generate SSH key pair.  
- Refer to video: https://www.youtube.com/watch?v=q9YA5H53IHQ  
- Follow the instructions to upload the public key to the server.


### 4. Disable password login for SSH
- **Do not perform this step**; it is informational only to understand SSH hardening. Changing this will block other students from accessing the lab server.
```bash
sudo nano /etc/ssh/sshd_config
# Set PasswordAuthentication no
sudo systemctl restart sshd
```

## SUBMISSION

### Step 1 — Email
- **To:** `support@ucubetech.com`  
- **Subject:** `CCT-MD-002 Submission - [Your Name]`
  
Attach the following files via email:  
- `CCT-FP-SH-005-suid.txt`  
- `CCT-FP-SH-005-sgid.txt`  
- `CCT-FP-SH-005-ssh-summary.txt` (summary of SSH configuration and verification)

- ### Step 2 — Microsoft Forms
- **Complete the form:** [https://forms.office.com/r/Pr4aYf6atB](https://forms.office.com/r/Pr4aYf6atB)
---
