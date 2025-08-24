## 🛠️ Day 1: Manual Splunk & UF Setup

**Objective:** Establish baseline Splunk architecture with Universal Forwarder (UF) for ingesting logs from lab machines.

**Reference:** [Manual Splunk and UF Setup Guide](https://www.notion.so/Manual-Splunk-and-UF-Set-up-255829fa6c4d80409910edf6bc1611d9)

### ✅ Tasks Completed
- Installed Splunk Enterprise on Ubuntu VM
- Configured Splunk Web UI access and admin credentials
- Deployed Universal Forwarder on Kali machine
- Verified forwarder connectivity via `splunk list forward-server`
- Enabled basic input stanzas for syslog and Suricata logs

### 🔍 Validation Steps
- Confirmed data ingestion via Splunk Search (`index=*`)
- Checked UF logs for successful handshakes
- Ensured Suricata alerts were visible in Splunk

### 🧠 Observations
- Manual UF setup reinforces understanding of deployment topology
- Initial ingestion pipeline is functional—ready for dashboarding

### 📸 Screenshot
![Splunk Search Results](https://copilot.microsoft.com/th/id/BCO.2947d3c6-ebc8-4207-9eba-6b28a18b3422.png)

## 🔐 Day 2: Investigating RDP Brute Force Attacks

**Objective:** Detect and analyze Remote Desktop Protocol (RDP) brute force attempts on Windows endpoints using Splunk.

**Reference:** [Manual: Investigating RDP Brute Force Attacks on Windows](https://www.notion.so/Manual-Investigating-RDP-Brute-Force-Attacks-on-Windows-250829fa6c4d80d6adccd8599ea0b066)

### ✅ Tasks Completed
- Identified relevant Windows EventCodes (e.g., 4625 for failed logins)
- Built Splunk search queries to isolate repeated login failures
- Applied time filters and aggregation to detect brute force patterns
- Mapped attacker IPs and usernames from raw event data

### 📊 Dashboard Progress
- Created panels for:
  - Failed login attempts over time
  - Top source IPs triggering 4625 events
  - Username frequency distribution
- Applied shared tokens for dynamic filtering

### 🧠 Observations
- Brute force detection hinges on correlating frequency and timing
- Splunk’s timechart and stats functions are ideal for pattern surfacing
- Next step: enrich with Suricata alerts for lateral movement detection

### 📸 Screenshot
![Sysmon Event Log showing suspicious file creation](https://github.com/weipei38/Portfolio/blob/main/Screenshot%202025-08-18%20165645.png?raw=true)

### 🧾 Artifact Highlight: Suspicious File Creation

The following Sysmon event shows `powershell.exe` creating a file named `micar.com.txt`—a strong indicator of post-exploitation activity:

- **Event Time:** 08/19/2025 12:02:56 AM  
- **Process:** `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`  
- **Target File:** `C:\Users\Administrator\Downloads\micar.com.txt`  
- **User:** `WIN-VJ19019120\Administrator`  
- **Event Source:** `Microsoft-Windows-Sysmon/Operational`  
- **Event Type:** `TaskCategory=File created (rule: FileCreate)`

This event can be correlated with earlier 4625 login failures to build a timeline of compromise.


## 🛡️ Day 3: RDP Brute Force Detection – Dashboard Refinement

**Objective:** Enhance visibility into RDP brute force attempts using Splunk dashboards and refine detection logic.

### ✅ Tasks Completed
- Tuned search queries to reduce noise from legitimate login failures
- Added conditional filters for EventCode 4625 with specific failure reasons
- Introduced correlation between source IP, username, and time window
- Validated detection logic using lab-generated attack traffic

### 📊 Dashboard Enhancements
- Added heatmap panel for login failures by hour
- Created drill-down panel for top offending IPs with user context
- Linked dashboard tokens to allow dynamic filtering across panels

### 📸 Screenshot
![RDP Brute Force Dashboard](https://github.com/weipei38/Portfolio/blob/main/Screenshot%202025-08-19%20075225.png?raw=true)

### 🧠 Observations
- Attackers often rotate usernames but reuse IPs—IP-based grouping is key
- Time-based clustering reveals attack bursts vs. slow probing
- Next step: enrich with Suricata alerts and lateral movement indicators

### 🧾 Artifact Highlight: IP/User Correlation Panel

This panel surfaces the top 10 source IPs with repeated login failures, grouped by username and timestamp. It helps distinguish targeted brute force from broad scanning.

- **Fields Used:** `src_ip`, `user`, `EventCode`, `_time`
- **Search Logic:** `stats count by src_ip, user | where count > 10`
- **Outcome:** Identified 3 IPs with >50 failed attempts in <5 minutes



---

