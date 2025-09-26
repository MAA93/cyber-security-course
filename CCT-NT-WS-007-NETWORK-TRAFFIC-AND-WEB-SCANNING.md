# DAY 7 - NETWORK TRAFFIC & WEB SCANNING | CCT-NT-WS-007
**Mode:** Discuss & document (practical elements on lab server / individual machines where possible)

---

## LEARN (20–25 min)
Network traffic capture + web application scanning are complementary skills: traffic capture (Wireshark/tcpdump) shows what flows across the network; web scanners (ZAP/Burp) test web application logic and inputs for vulnerabilities.

**Key concepts**
- Passive vs active analysis  
- Packet capture (pcap) basics: frames, packets, protocols, ports  
- Intercepting HTTP(S) with a proxy (Burp/ZAP)  
- Active web scanning (injecting payloads to test XSS/SQLi/etc) vs passive observation  
- How Nmap + NSE complements app-level scanners (service/version/vuln discovery)

---

## RESOURCES
- Nmap: https://nmap.org/book/man.html  
- Nmap NSE vuln scripts: https://nmap.org/nsedoc/  
- OWASP ZAP: https://www.zaproxy.org/getting-started/  
- Burp Suite docs: https://portswigger.net/burp/documentation  
- Wireshark User Guide: https://www.wireshark.org/docs/  
- Packet capture basics: https://www.tcpdump.org/manpages/tcpdump.1.html

Optional (for instructor demo / further study):
- OpenVAS / Nessus, Metasploit, Snort/Suricata, Amass, Shodan

---

## SAFETY & ETHICS (READ FIRST)
- **Only run active scans (Nmap --script, ZAP active scan, Burp active scan) against lab/test servers that you own or have explicit permission to test.** Doing otherwise may be illegal.  
- Active scans generate traffic and may cause instability—avoid scanning production systems.  
- When using proxies (ZAP/Burp), ensure HTTPS interception is configured correctly and remove custom CA certs from the browser after the lab if you installed them.

---

## DO (45–60 min)

> NOTE: Some steps require GUI tools (Burp Suite, OWASP ZAP, Wireshark). Students on Windows or Linux can run these locally; the lab server is used as the target.

### A. SSH into your lab server (for server-side commands / scans)
```bash
ssh [your-username]@[your-server-ip]
```
---

### B. Nmap — host & vuln discovery (quick examples)
# Scan localhost quickly (SYN scan, fast)
```bash
nmap -sS -F localhost -oN CCT-NT-WS-007-nmap-localhost.txt
```
# Scan remote lab server from your machine (full ports)
```bash
nmap -sS -p- -T4 [lab-server-ip] -oN CCT-NT-WS-007-nmap-server-full.txt
```
# Service/version detection + default scripts
```bash
nmap -sV -sC [lab-server-ip] -oN CCT-NT-WS-007-nmap-scripts.txt
```
# Vulnerability scripts (NSE vuln category)
```bash
nmap --script vuln [lab-server-ip] -oN CCT-NT-WS-007-nmap-vuln.txt
```
**What to look for:** open ports, services, versions, NSE script findings (CVE info or weakness indicators).

---

### C. Capture network traffic (tcpdump → Wireshark)
# Capture 500 packets on the primary interface (replace eth0 with correct interface)
```bash
sudo tcpdump -i eth0 -c 500 -w CCT-NT-WS-007-capture.pcap
```
# Quick summary of capture (in terminal)
```bash
tcpdump -r CCT-NT-WS-007-capture.pcap -nn -tt | head -n 50 > CCT-NT-WS-007-capture-summary.txt
```
- Open CCT-NT-WS-007-capture.pcap in Wireshark to:
  - Filter (example): ip.addr == [lab-server-ip] or http or tcp.port == 80
  - Inspect individual packets, follow TCP streams, view HTTP requests/responses
  - Identify plaintext credentials (HTTP), unusual protocols, recon scans (many SYNs), or large data exfiltration patterns

---

### D. Web scanning — Burp Suite & OWASP ZAP (intercept + active scan)

**Setup (both tools)**
- Configure your browser to use proxy: 127.0.0.1:8080 (or tool default)  
- Import the proxy CA cert into your browser (only for lab work). Remove it after lab.

**OWASP ZAP (recommended free option)**
1. Start ZAP → set proxy in browser.  
2. Browse the lab web app to populate the Sites tree (or use ZAP Spider).  
3. Run Passive Scan (observes headers, cookies) — safe.  
4. Start Active Scan on selected nodes (this will send payloads).  
5. Export report: CCT-NT-WS-007-zap.txt or HTML export.

**Burp Suite (Community / Professional)**
1. Start Burp → Proxy → intercept traffic while browsing.  
2. Use Repeater to manually modify and re-send requests for testing.  
3. Use Intruder for controlled automated payloads (careful—can be noisy).  
4. If you have Burp Scanner (Pro), run Active Scan and export results: CCT-NT-WS-007-burp.txt.

**Important classroom note:** Active scans should target only the lab server. Instructor may perform heavy scans in demonstration.

---

### F. Analyze & Document
Create CCT-NT-WS-007-analysis.txt and include:
- Nmap findings (open ports, service versions, notable script results)  
- Evidence from Wireshark (suspicious packets, example tcp streams)  
- ZAP/Burp findings (high/medium issues, request/response snippets)  
- Short remediation recommendations (patch versions, remove unused services, harden headers, input validation)

Example analysis commands to capture snippets:
# Top 10 frequent source IPs in pcap (using tshark)
```bash
tshark -r CCT-NT-WS-007-capture.pcap -T fields -e ip.src | sort | uniq -c | sort -nr | head -n 10 > CCT-NT-WS-007-top-src-ips.txt
```
---

## SUBMISSION
Email to: support@ucubetech.com  
**Subject:** CCT-NT-WS-007 Submission - [Your Name]  
Attach:
- CCT-NT-WS-007-analysis.txt

— Microsoft Forms
- **Complete the form:** [https://forms.office.com/r/Pr4aYf6atB](https://forms.office.com/r/Pr4aYf6atB)
---

## TROUBLESHOOTING & TIPS (for students)
- If Wireshark cannot capture on Linux due to permissions, run with sudo or ensure your user is in the wireshark group.  
- Browser not trusting ZAP/Burp CA? Re-import the CA certificate (only for lab) and remove it after.  
- If scans take long, narrow scope (specific path or limited port list).  
- If server appears unresponsive after scan, stop the scan and notify instructor.


---

## GOAL
By the end of Day 7 students will be able to:
- Capture and inspect network traffic relevant to web applications.  
- Run Nmap to discover hosts/services and run vulnerability NSE scripts.  
- Use OWASP ZAP or Burp Suite to intercept and actively test web app endpoints (in lab).  
- Correlate application-level scans with network-level evidence and write a short analyst-style report.
