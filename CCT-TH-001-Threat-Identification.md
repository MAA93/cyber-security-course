# DAY 1 - THREAT IDENTIFICATION | TASK ID: CCT-TH-001

## Objective
Transition from theory to practice by identifying real-world threat indicators in live system logs. This is a fundamental skill for any cybersecurity analyst.

## 1. LEARN (15 min)

A **threat** is any potential danger to an asset. Threat sources can be:
- **Natural:** Floods, earthquakes, fires.
- **Unintentional Human Error:** Accidentally deleting a critical file or misconfiguring a firewall.
- **Intentional Attacks:** Malicious actions from internal or external actors, like a hacker trying to breach a system.

Today, we focus on identifying evidence of **intentional attacks**.

### Resources
- **Watch:** [Types of Cyber Threats](https://youtu.be/Dk-ZqQ-bfy4) (12 min)
- **Reference:** [NIST SP 800-30 Guide](https://csrc.nist.gov/pubs/sp/800/30/r1/final) (For your reference)

## 2. DO (20 min) - Hands-On Lab

Follow these steps to find evidence of failed intrusion attempts.

1.  **Access Your Lab Server:**
    ```bash
    ssh student@[your-server-ip]
    ```
    *Password will be provided by the instructor.*

2.  **Examine Authentication Logs:** Check the last 50 lines of the auth log.
    ```bash
    sudo tail -50 /var/log/auth.log
    ```

3.  **Look for Threats:** Scan for lines containing `Failed password` or `Invalid user`.

4.  **Document Your Findings:** Create an analysis file.
    ```bash
    nano CCT-TH-001-threat-analysis.txt
    ```
    In the file, note:
    - The number of failed login attempts.
    - Timestamps of the attempts.
    - IP addresses of the attackers.
    - Usernames that were targeted (e.g., `root`, `admin`).

5.  **Take a Screenshot:** Capture a clear screenshot of your terminal showing the output of the `sudo tail -50 /var/log/auth.log` command.

## 3. SUBMIT (By End of Class)

To receive credit, you must complete **BOTH** steps below before the end of today's session.

### STEP 1: Email Submission
- **Send to:** `support@ucubetech.com`
- **Subject:** `CCT-TH-001 Submission - [Your Name]`
- **Body:**
    ```
    Dear Instructor,

    Please find my submission for Task CCT-TH-001 attached.

    Name: [Your Name]
    Thank you.
    ```
- **Attachments:**
    - `CCT-TH-001-threat-analysis.txt`
    - Your terminal screenshot.

### STEP 2: Microsoft Forms Submission
- **Complete the form:** [https://forms.office.com/r/Pr4aYf6atB](https://forms.office.com/r/Pr4aYf6atB)

## Goal
Successfully identify, document, and submit evidence of threat indicators from system logs.
