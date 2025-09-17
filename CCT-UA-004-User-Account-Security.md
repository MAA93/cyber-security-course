# DAY 4 - USER ACCOUNT SECURITY | TASK ID: CCT-UA-004

## 📖 LEARN (15 min)
Linux stores user accounts in `/etc/passwd` and password hashes in `/etc/shadow`. Weak passwords, accounts without proper restrictions, or unused accounts can create vulnerabilities. Some accounts may exist on the lab server that are intentionally misconfigured to help you practice auditing.

## 📚 RESOURCES
- [Understanding /etc/passwd format](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/)
- ["Linux User Management"](https://youtu.be/19WOD84JFxA) (18 min video)
- [Password policies in RHEL 8 & 9](https://access.redhat.com/solutions/5027331?utm_source=chatgpt.com)
- Optional: [Linux password aging explained](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/4/html/security_guide/s3-wstation-pass-org-age?utm_source=chatgpt.com)

---

## 🔨 DO (20–25 min)

1. **List all users**
   ```bash
   cat /etc/passwd > CCT-UA-004-users.txt
   ```

2. **Check for empty passwords**
   ```bash
   sudo awk -F: '($2 == "" ) {print $1}' /etc/shadow > CCT-UA-004-empty-passwords.txt
   ```
   ⚠️ Some accounts on the lab server may have no password. Identify them carefully.

3. **Create a secure test user**
   ```bash
   sudo useradd -m -s /bin/bash [new-username]
   sudo passwd [new-username]
   ```
   - Use a **strong password** (uppercase, lowercase, numbers, special symbols).

4. **Document password policies**
   ```bash
   grep PASS /etc/login.defs > CCT-UA-004-password-policy.txt
   ```
   Modern Linux systems may also use **PAM** (`/etc/security/pwquality.conf`) for password quality checks.

5. **Create security report**
   ```bash
   nano CCT-UA-004-security-report.txt
   ```
   Suggested points to include:
   - Number of users found in `/etc/passwd`
   - Any accounts with empty passwords
   - Any accounts with UID 0 other than root
   - Weak passwords or policy issues
   - Did you successfully create a secure test user?
   - Recommendations for securing accounts

---

## ✅ SUBMIT

- Email the following files to `support@ucubetech.com`:
  - `CCT-UA-004-users.txt`
  - `CCT-UA-004-empty-passwords.txt`
  - `CCT-UA-004-password-policy.txt`
  - `CCT-UA-004-security-report.txt`
    
Complete the form: https://forms.office.com/r/Pr4aYf6atB

---

## 🎯 GOAL
- Audit Linux user accounts and passwords  
- Detect hidden misconfigurations on the lab server  
- Learn to create secure accounts and enforce strong password policies  
- Practice documenting security findings like a cyber security analyst
