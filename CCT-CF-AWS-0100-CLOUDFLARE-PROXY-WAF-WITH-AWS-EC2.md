# DAY 10 – CLOUDFLARE PROXY & WAF WITH AWS EC2 | CCT-CF-AWS-010

## 🎯 Learning Objectives
- Understand how **Cloudflare** protects web applications hosted on **AWS EC2**.  
- Learn how to configure **AWS Security Groups** to allow only Cloudflare traffic.  
- Explore **WAF (Web Application Firewall)** concepts, modes, and alternatives.  
- Gain skills to verify if Cloudflare is **actively proxying** traffic.  


## 📘 Key Concepts

| Concept | Explanation | Example / Notes |
|---------|-------------|-----------------|
| **Reverse Proxy** | Cloudflare sits between users and EC2, hiding the real server IP. | Protects from DDoS and bot attacks. |
| **AWS Security Group Adjustment** | Limit inbound to Cloudflare IPs only. | Published at [Cloudflare IPs](https://www.cloudflare.com/ips). |
| **WAF** | Protects against OWASP Top 10, SQLi, XSS, brute force. | Both Cloudflare WAF and AWS WAF available. |
| **WAF Modes** | - Monitoring (log only)<br>- Blocking (actively deny)<br>- Learning/AI mode | Start in monitoring, then block. |
| **Verification** | Ensure traffic passes Cloudflare, not direct EC2. | Look for `cf-ray` header, logs show only CF IPs. |

---

## 🧑‍🏫 Q&A

### Q1. Why do we put Cloudflare in front of an AWS EC2 instance?  
**Answer:** Cloudflare is a reverse proxy that adds security, performance, and resilience.  

---

### Q2. What changes do we need in AWS Security Groups when using Cloudflare?  
**Answer:** allow **only Cloudflare IP ranges**. This prevents attackers from bypassing Cloudflare.  

---

### Q3. What does a Web Application Firewall (WAF) protect against?  
**Answer:**  WAF mitigates OWASP Top 10 attacks and blocks bad bots.  

---

### Q4. Cloudflare WAF vs AWS WAF — which is better?  
**Answer:**
- **Cloudflare WAF:** DNS-level, easier, free tier available.  
- **AWS WAF:** More granular, integrated with ALB/API Gateway.  
- Use Cloudflare for **quick deployments**, AWS WAF for **AWS-heavy apps**.  

---

### Q5. What modes can a WAF operate in?  
| Mode | Behavior | When to Use |
|------|----------|-------------|
| **Monitoring / Logging** | Detects & logs suspicious traffic. | During testing / learning. |
| **Blocking** | Actively blocks malicious traffic. | After testing & validation. |
| **AI / Learning Mode** | Learns normal traffic patterns. | Advanced enterprise setups. |

---

### Q6. What other WAF solutions exist?  
| Type | Tools |
|------|-------|
| **Open Source** | ModSecurity + OWASP CRS |
| **Cloud / SaaS** | Cloudflare WAF, Akamai Kona, Fastly WAF |
| **Enterprise** | Imperva WAF, F5 Advanced WAF |

---

### Q7. How do you verify Cloudflare is proxying your traffic?  
**Expected Student Thoughts:** DNS, headers, logs.  
**Instructor Answer:**  
- DNS should resolve to **Cloudflare IPs**.  
- Response headers show `cf-ray`, `cf-cache-status`.  
- EC2 access logs show only Cloudflare IPs (real client IP in `CF-Connecting-IP`).  

---

## 🔍 Brainstorming Scenarios

| Scenario | Question | Answer |
|----------|------------------|---------------------------|
| **Bypassing Cloudflare** | "If attacker gets EC2 IP, can they bypass CF?" | Yes, unless EC2 SG only allows Cloudflare IPs. |
| **WAF in Monitor Mode** | "What if WAF is in monitoring only?" | Attacks logged, but not blocked → useful for testing. |
| **Multiple WAFs** | "Can we use both CF WAF + AWS WAF?" | Yes — layered defense ("defense in depth"). |
| **Performance Impact** | "Does proxying slow traffic?" | Sometimes, but caching/optimization offsets this. |

---

## 🛠️ Hands-On Demo Flow (Nothing to be done here, just for your information)

1. **Step 1:** Deploy a basic EC2 instance (Ubuntu, Nginx/Apache).  
2. **Step 2:** Point domain DNS to Cloudflare → enable **proxy (orange cloud)**.  
3. **Step 3:** Test direct IP access (should be blocked by SG).  
4. **Step 4:** Restrict EC2 Security Group → only allow Cloudflare IP ranges.  
5. **Step 5:** Enable **Cloudflare WAF** → test SQL injection attempt (`' OR 1=1 --`).  
6. **Step 6:** Show logs on EC2 (`/var/log/nginx/access.log`) → verify only Cloudflare IPs visible.  


## SUBMISSION

### Step 1 — Email
- **To:** `support@ucubetech.com`  
- **Subject:** `CCT-CF-AWS-0100 Submission - [Your Name]`
  
Attach the following files via email:  
- `CCT-CF-AWS-0100-summary.txt` (summary of what you have learned)

- ### Step 2 — Microsoft Forms
- **Complete the form:** [https://forms.office.com/r/Pr4aYf6atB](https://forms.office.com/r/Pr4aYf6atB)
---

