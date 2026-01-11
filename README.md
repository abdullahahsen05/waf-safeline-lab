# Web Application Firewall (WAF) Lab with SafeLine

## 📌 Project Overview
This project demonstrates the deployment of **SafeLine WAF** (by Chaitin) to protect a vulnerable web application (DVWA). The goal was to simulate real-world web attacks (SQL Injection, DoS) and configure the WAF to detect and block them in a **Reverse Proxy** architecture.

## 🛠️ Architecture
* **WAF Engine:** SafeLine (Chaitin) running on Ubuntu (Docker container).
* **Target Application:** DVWA (Damn Vulnerable Web App) running on Apache/MySQL.
* **Attacker Machine:** Kali Linux.
* **Traffic Flow:**
    `User/Attacker (HTTPS)` -> `SafeLine WAF (Port 443)` -> `Backend Server (Port 8080)`

## ⚙️ Configuration Highlights

### 1. Reverse Proxy & SSL Offloading
* Configured SafeLine to listen on **Port 443** (HTTPS) and forward traffic to the backend on **Port 8080**.
* Generated and installed **Self-Signed Certificates** (OpenSSL) to enforce encrypted connections, redirecting all HTTP traffic to HTTPS.

### 2. Rate Limiting (DoS Protection)
* **Objective:** Prevent HTTP Flood attacks.
* **Rule:** Block any IP making more than **3 requests in 10 seconds**.
* **Result:** Launched a flood attack from Kali; the WAF automatically banned the attacker IP for 5 minutes.

## 🛡️ Attack Simulation & Defense

### Scenario 1: SQL Injection (SQLi)
* **Attack:** Attempted a standard SQL injection query: `' OR '1'='1`.
* **Outcome:** The WAF intercepted the malicious payload in the URL parameter and returned a **403 Forbidden** response.
* **Log Verification:** The SafeLine dashboard categorized the event as a "High Severity SQL Injection" attempt.

### Scenario 2: IP Blacklisting
* **Defense:** Created a custom rule to strictly **DENY** traffic from the Kali Linux IP (192.168.0.206).
* **Outcome:** All requests were immediately dropped, effectively isolating the threat actor.
* 
## 📸 Proof of Concept
### 1. WAF Dashboard & Traffic Stats
![WAF Dashboard](WAF-ScreenShots/waf_dashboard.png)
*(Overview of legitimate vs. blocked traffic)*

### 2. SQL Injection Blocked Page
![Blocked Request](WAF-ScreenShots/sqli_blocked.png)
*(The custom 403 error page served by SafeLine when an attack is detected)*

### 3. Incident Logs
![Attack Logs](WAF-ScreenShots/attack_logs.png)
*(Detailed log showing the attacker IP, timestamp, and the specific SQL payload caught)*### 1. WAF Dashboard & Traffic Stats
![WAF Dashboard](screenshots/waf_dashboard.png)
*(Overview of legitimate vs. blocked traffic)*

### 2. SQL Injection Blocked Page
![Blocked Request](screenshots/sqli_blocked.png)
*(The custom 403 error page served by SafeLine when an attack is detected)*

### 3. Incident Logs
![Attack Logs](screenshots/attack_logs.png)
*(Detailed log showing the attacker IP, timestamp, and the specific SQL payload caught)*

## 🚀 Key Takeaways
* Understanding **Reverse Proxy** deployment for WAFs.
* Practical experience with **SSL Certificate management**.
* Analyzing Layer 7 logs to distinguish between normal traffic and malicious payloads.
