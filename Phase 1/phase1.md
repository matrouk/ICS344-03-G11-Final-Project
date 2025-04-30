---

# King Fahd University of Petroleum & Minerals  
**Information and Computer Science Department**

---

## ICS344: Information Security

# Project Phase #2
---

## 1. Overview

In this phase, we integrated logs from the victim machine (Metasploitable3) into Splunk on our attacker machine (Kali Linux) to analyze the results of our SSH brute-force attack. The logs were collected using Splunk Universal Forwarder and visualized through Splunk’s dashboard.

---

## 2. Environment Setup

| Component         | Description                      |
|-------------------|----------------------------------|
| Victim Machine    | Metasploitable3 (192.168.0.181)  |
| Attacker Machine  | Kali Linux (192.168.0.232)       |
| SIEM Tool         | Splunk Enterprise                |
| Forwarding Tool   | Splunk Universal Forwarder       |
| Log File          | /var/log/auth.log                |
| Targeted Service  | SSH                              |

> Followed the steps in the GitHub and installed Splunk server in Kali and Splunk forwarder in Metasploitable.

---

## 3. Configure Splunk Receiver (Kali Linux)

### Step 1: Enabled Receiving on Splunk

- Opened browser in Kali:  
  `http://hussain:8000`

- Logged in with the credentials set up during Splunk server installation:  
  - **Username:** `hussain`  
  - **Password:** `IloveRain123`

- Navigated to:  
  **Settings → Forwarding and Receiving → Configure Receiving**

- Clicked on **New Receiving Port**, and set:  
  - **Port:** `9997`

---

## 4. Configure Forwarder on Victim (Metasploitable3)

### Step 1: Connect to Splunk Server from Metasploitable

```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server 192.168.0.232:9997
```

### Step 2: Add Logs to Monitor

```bash
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log
```

### Step 3: Restart Forwarder

```bash
sudo /opt/splunkforwarder/bin/splunk restart
```

### Step 4: Confirm Forwarder Status

```bash
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

---

## 5. Attack Execution (Brute Force) using Metasploit (Kali)

We used Metasploit to generate SSH login attempts on the victim using brute force, similar to phase 1.

**Commands:**
```bash
sudo msfconsole
use auxiliary/scanner/ssh/ssh_login
set RHOSTS 192.168.0.181
set USER_FILE usernames.txt && set PASS_FILE passwords.txt
set VERBOSE true
run
```

---

## 6. Log Analysis in Splunk

- Opened **Splunk Search**:  
  `Search & Reporting → Data Summary → Sources → metasploitable`

- Located `/var/log/auth.log` and selected it as the source.

- Auth logs appeared, showing login attempts.

---

## 7. Analyze SSH Attack Patterns

- Used the following search query to show login events:

    ```
    "Failed password" OR "Accepted password"
    ```

  This query displays all brute-force login attempts detected by SSH.

### Visualization

- Clicked on the **Visualization** tab and switched to:
  - **Timechart → Failed logins over time**
  - **X-axis:** _time  
  - **Y-axis:** count

---

## 8. Summary & Findings

- Successfully configured Splunk to receive logs from victim
- Executed real SSH attacks using Metasploit
- Forwarded and visualized logs of brute-force attempts
- Observed attack patterns and validated system compromise

---

Let me know if you want **further formatting, images, or anything extra** (e.g., table of contents, headings, etc.)!