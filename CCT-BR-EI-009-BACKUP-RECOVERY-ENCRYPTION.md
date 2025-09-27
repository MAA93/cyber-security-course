# DAY 9 - BACKUP, RECOVERY & ENCRYPTION | CCT-BR-EI-009
**Mode:** Practical (lab server / local machine). Configure backups, test recovery, and demonstrate encryption using safe methods.

---

### 1) SSH into your lab server
```bash
ssh [your-username]@[your-server-ip]
```

---

### 2) Simple rsync backup (local test)
```bash
mkdir -p $HOME/backups/rsync-$(date +%F)
sudo rsync -aAXv --delete /var/www/html/ \
  $HOME/backups/rsync-$(date +%F)/www-html/ \
  > CCT-BR-EI-009-rsync-log.txt 2>&1
```
*Explanation:*  
- Creates a dated backup directory.  
- `rsync -aAXv --delete` copies files preserving permissions and ACLs, and removes deleted files from the destination.  
- Logs output for documentation.

```bash
ls -l $HOME/backups/rsync-$(date +%F)/www-html/ | head -n 50 \
  > CCT-BR-EI-009-rsync-listing.txt
```
*Explanation:* Lists the first 50 files in the backup for verification.

---

### 3) BorgBackup (deduplicated backup)
```bash
sudo apt install -y borgbackup
borg init --encryption=repokey $HOME/backups/borg-repo
borg create --stats $HOME/backups/borg-repo::$(date +%F) /var/www/html/
borg list $HOME/backups/borg-repo > CCT-BR-EI-009-borg-archive.txt
```
*Explanation:*  
- `borg init` creates a secure Borg repository.  
- `borg create` backs up `/var/www/html/`.  
- `borg list` verifies archives for submission.

---

### 4) GPG file encryption (choose a real file)
```bash
# Automatically pick the first file in the backup directory
FILE_TO_ENCRYPT=$(ls $HOME/backups/rsync-$(date +%F)/www-html/ | head -n 1)
gpg --batch --yes --passphrase "StrongPass123" -c \
  "$HOME/backups/rsync-$(date +%F)/www-html/$FILE_TO_ENCRYPT"
ls -l $HOME/backups/rsync-$(date +%F)/www-html/*.gpg > CCT-BR-EI-009-gpg-encrypt.txt
```
*Explanation:*  
- `FILE_TO_ENCRYPT` dynamically selects the first file in the backup directory.  
- `gpg -c` encrypts it with a passphrase.  
- Listing `.gpg` files ensures verification and documentation for submission.


---

### 5) Restore verification
```bash
# Restore rsync backup to test directory
mkdir -p $HOME/restore-test
sudo rsync -aAXv $HOME/backups/rsync-$(date +%F)/www-html/ $HOME/restore-test/ \
  > CCT-BR-EI-009-restore-verification.txt 2>&1
```
*Explanation:*  
- Restores backup to a temporary directory to verify completeness.  
- Logs output for submission.

---

## SUBMISSION — DAY 9

### Step 1 — Email
*Explanation:* Submit all evidence via email with required files attached.

- **To:** `support@ucubetech.com`  
- **Subject:** `CCT-BR-EI-009 Submission - [Your Name]`  

**Attach:**  
- `CCT-BR-EI-009-rsync-log.txt`  
- `CCT-BR-EI-009-rsync-listing.txt`  
- `CCT-BR-EI-009-borg-archive.txt`  
- `CCT-BR-EI-009-gpg-encrypt.txt`  
- `CCT-BR-EI-009-restore-verification.txt`

---

### Step 2 — Microsoft Forms
*Explanation:* Form submission provides instructors with a summary of your work.

- **Complete the form:** [https://forms.office.com/r/Pr4aYf6atB](https://forms.office.com/r/Pr4aYf6atB)  
- Provide your name, lab server IP (if required), and short descriptions of each backup/encryption task.  
- Reference or attach the same files you emailed if requested.
