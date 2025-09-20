# Day 6 – Firewall & Log Monitoring | CCT-FW-LM-006

## Objectives
- Configure and manage a firewall on Ubuntu (UFW).  
- Monitor logs for SSH and system security.  
- Install and use Fail2Ban to protect against brute-force attacks.  
- Document findings and understand practical steps for system hardening.

---

## Learn (20 min)

Firewalls filter network traffic and prevent unauthorized access. **UFW** is the default firewall for Ubuntu. Proper configuration ensures only required services are exposed.  

System logs record critical events such as login attempts, service starts/stops, and security alerts. Monitoring these logs helps detect attacks early.  

**Key concepts:**
- UFW basics: enable, allow, deny, default policies.  
- SSH traffic control.  
- Log files: `/var/log/auth.log`, `/var/log/syslog`.  
- Fail2Ban: automated log monitoring and temporary IP banning.  

---

## Resources
- UFW Ubuntu Documentation – https://help.ubuntu.com/community/UFW  
- Fail2Ban Documentation – https://www.fail2ban.org/wiki/index.php/Main_Page  
- Linux Log Files Explained – https://www.loggly.com/ultimate-guide/linux-logging-basics/  
- SSH Security Logs – https://signoz.io/guides/ssh-logs/

---

## Do (45 min)

### 1. Enable and Configure Firewall (UFW)

Enable UFW:
```bash
sudo ufw enable
```

Check current rules:
```bash
sudo ufw status verbose
```

Allow SSH (port 22):
```bash
sudo ufw allow ssh
sudo ufw allow 22/tcp
```

Allow web traffic:
```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

Set default policies:
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Verify:
```bash
sudo ufw status numbered
```

---

### 2. Check SSH Logs

SSH logs are in `/var/log/auth.log`.

Failed login attempts:
```bash
sudo grep "Failed password" /var/log/auth.log
```

Successful logins:
```bash
sudo grep "Accepted password" /var/log/auth.log
```

All SSH events:
```bash
sudo grep sshd /var/log/auth.log
```

Monitor logs in real-time:
```bash
sudo tail -f /var/log/auth.log
```

Check last login attempts:
```bash
last
lastb  # failed logins (requires root)
```

---

### 3. Install and Configure Fail2Ban (Skip--already installed)

Fail2Ban automatically bans IP addresses that show malicious signs (e.g., repeated failed SSH logins).

Install:
```bash
sudo apt update
sudo apt install fail2ban -y
```

Check status:
```bash
sudo systemctl status fail2ban
```

Default jail configuration (protect SSH):
```bash
sudo nano /etc/fail2ban/jail.local
```
Example configuration:
```
[sshd]
enabled = true
port    = ssh
filter  = sshd
logpath = /var/log/auth.log
maxretry = 5
bantime  = 3600
```

Restart Fail2Ban to apply changes:
```bash
sudo systemctl restart fail2ban
```

Check banned IPs:
```bash
sudo fail2ban-client status sshd
```

Unban IP if needed:
```bash
sudo fail2ban-client set sshd unbanip <IP_ADDRESS>
```

---

### 4. Hands-On Exercises

1. Enable UFW and configure it to allow **only SSH, HTTP, and HTTPS**.  
2. Block all other incoming traffic.  
3. Review `/var/log/auth.log` and identify at least **3 failed login attempts**.  
4. Install Fail2Ban, enable SSH protection, and confirm **any active bans**.  
5. Summarize findings in a report.

---

## Submission

### Step 1 — Email
- **To:** `support@ucubetech.com`  
- **Subject:** `CCT-FW-LM-006 Submission - [Your Name]`  
- **Attachments:**  
- **CCT-FW-LM-006-summary.txt** – summary of what you have learned from this task`  


### Step 2 — Microsoft Forms
- **Complete the form:** [https://forms.office.com/r/Pr4aYf6atB](https://forms.office.com/r/Pr4aYf6atB)
---

Students must submit:


---

## Notes

- Do **not** lock yourself out of your server; always test SSH access before closing your current session.  
- Use a second terminal or session to avoid being cut off.  
- Fail2Ban provides **automatic temporary bans** to mitigate brute-force attacks.  

