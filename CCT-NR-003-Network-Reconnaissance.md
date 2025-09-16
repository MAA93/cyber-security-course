# DAY 3 - NETWORK RECONNAISSANCE (Network Focus) | TASK ID: CCT-NR-003

> **Run this task on the remote lab server**.  
> SSH to the lab server and execute the commands there so scans are **local** to the target host.  
> **Always have explicit permission to run scans. Unauthorized scanning is illegal.**

---

## LEARN (10–15 min)
Network reconnaissance is discovering which services are exposed.  
We’ll use **Nmap** for port scanning (main tool) and optionally compare with **ss** to confirm which services are listening.

Common ports:
- 22 → SSH  
- 80 → HTTP  
- 443 → HTTPS  
- 3306 → MySQL  

---

## TOOLS USED (Day 3)
- **nmap** → network/port scanner  
- **ss** → show listening sockets (optional cross-check only)

---

## DO (25–30 min)

### 0) SSH into your lab server
```bash
ssh [your-username]@[your-server-ip]
```

### 1) Install tool (on remote server) (Skip--already installed)
```bash
sudo apt update
sudo apt install -y nmap
```

### 2) Create results folder
```bash
mkdir -p $HOME/CCT-NR-003-results
cd $HOME/CCT-NR-003-results
```

### 3) Run scans (on remote server)
```bash
# Quick SYN scan of common ports
nmap -sS -F localhost -oN nmap-quick.txt

# Full TCP port scan (optional — may take longer)
nmap -sS -p- localhost -oN nmap-full.txt
```

### 4) OPTIONAL cross-check with ss
```bash
sudo ss -tuln > ss-listening.txt 2>&1
```

---

## ANALYZE
Create a file:
```bash
nano $HOME/CCT-NR-003-results/CCT-NR-003-analysis.txt
```

Answer:
- Which ports are open in the quick scan?  
- Did the full scan find more?  
- Do the results match what `ss` shows?  
- Which services should be hardened or restricted?  

---

## SUBMISSION
Send to: `support@ucubetech.com`  
Subject: `CCT-NR-003 Submission - [Your Name]`  

Attach:
- `nmap-quick.txt`  
- `nmap-full.txt` (if run)  
- `ss-listening.txt` (if run)  
- `CCT-NR-003-analysis.txt`

Also complete the Microsoft Form for Day 3:  [https://forms.office.com/r/Pr4aYf6atB](https://forms.office.com/r/Pr4aYf6atB)

---

## OPTIONAL — Scan from your own machine
If you want to practice scanning remotely:

### From Linux
```bash
nmap -sS -F [your-server-ip] -oN nmap-from-home.txt
```

### From Windows (requires Npcap)
```powershell
nmap -sS -F SERVER_IP -oN nmap-from-windows.txt
```

---

## SAFETY
- Only scan systems you are authorized to test.  
- Do not scan beyond the lab server.  
- Only scan systems you are authorized to test.  
- Do not scan beyond the lab server.  
