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

## 🧾 Day 4: File Integrity Monitoring with auditd

**Objective:** Detect unauthorized file modifications in sensitive Linux directories using `auditd` logs ingested into Splunk.

**Reference:** [Manual: File Integrity Monitoring for Sensitive Directories](https://www.notion.so/Manual-File-Integrity-Monitoring-for-Sensitive-Directories-255829fa6c4d8024a1dffd5ffbb5e720)

### ✅ Tasks Completed
- Enabled `auditd` rules to monitor file creation, deletion, and permission changes in `/etc`, `/var/log`, and `/home`
- Verified log ingestion into Splunk under `sourcetype="file_integrity"`
- Built Splunk queries to surface suspicious file access and modifications
- Flagged unauthorized changes to config files and dropped artifacts in monitored paths

### 📊 Dashboard Progress
- Panels created for:
  - File access events by path and syscall
  - Top modified files by user and host
  - Permission changes and deletions
- Applied filters for `syscall=creat`, `chmod`, `unlink`, and `open`

### 📸 Screenshot
![Day 4 - File Integrity Monitoring](https://github.com/weipei38/Portfolio/blob/main/Screenshot%202025-08-21%20233304.png?raw=true)

### 🧠 Observations
- `auditd` provides granular visibility into file-level operations
- Monitoring `/etc` and `/var/log` is critical for early compromise detection
- File drops in `/tmp` and `/home` often signal staging or exfiltration prep

### 🧪 Artifact Highlight: Suspicious File Creation

Detected creation of `micar.txt` in `/home/administrator/Downloads` via `auditd`:

- **Event Time:** 08/19/2025 12:02:56 AM  
- **Syscall:** `creat`  
- **Target File:** `/home/administrator/Downloads/micar.txt`  
- **User:** `administrator`  
- **Sourcetype:** `file_integrity`  
- **Event Type:** File created

This event correlates with earlier brute force attempts and post-exploitation behavior observed in Day 2 and Day 3.

## 🧾 Day 4: File Integrity Monitoring for Sensitive Directories

**Objective:** Detect unauthorized file modifications in sensitive Linux directories using `auditd` and Suricata logs in Splunk.

**Reference:** [Manual: File Integrity Monitoring for Sensitive Directories](https://www.notion.so/Manual-File-Integrity-Monitoring-for-Sensitive-Directories-255829fa6c4d8024a1dffd5ffbb5e720)

### ✅ Tasks Completed
- Enabled `auditd` rules to monitor file creation, deletion, and permission changes in `/etc`, `/var/log`, and `/home`
- Verified log ingestion into Splunk under `sourcetype="file_integrity"`
- Correlated file creation events with Suricata alerts for post-exploitation behavior
- Flagged unauthorized changes to config files and dropped artifacts in monitored paths

### 📊 Dashboard Progress
- Panels created for:
  - File access events by path and syscall
  - Top modified files by user and host
  - Suricata alerts tied to file drops and suspicious flows
- Applied filters for `syscall=creat`, `chmod`, `unlink`, and `event_type=alert`

### 📸 Screenshot
![Day 4 - Suricata File Integrity](https://github.com/weipei38/Portfolio/blob/main/Screenshot%202025-08-24%20143942.png?raw=true)

### 🧠 Observations
- `auditd` provides granular visibility into file-level operations
- Suricata alerts help validate whether file drops are benign or part of a larger compromise
- Monitoring `/etc`, `/var/log`, and `/home` is critical for early detection

### 🧪 Artifact Highlight: Suspicious File Creation

Detected creation of `micar.txt` in `/home/administrator/Downloads` via `auditd`:

- **Event Time:** 08/19/2025 12:02:56 AM  
- **Syscall:** `creat`  
- **Target File:** `/home/administrator/Downloads/micar.txt`  
- **User:** `administrator`  
- **Sourcetype:** `file_integrity`  
- **Event Type:** File created

Suricata flagged related outbound traffic shortly after the file drop, suggesting staging behavior.

## 🔥 Day 6: Palo Alto Firewall Log Analysis

**Objective:** Detect policy violations, threat signatures, and outbound anomalies using Palo Alto firewall logs in Splunk.

**Reference:** [Manual: Palo Alto Firewall Log Analysis](https://www.notion.so/Manual-Palo-Alto-Firewall-Log-Analysis-255829fa6c4d8064b08ff657a8463cde)

### ✅ Tasks Completed
- Ingested Palo Alto logs into Splunk (`sourcetype="pan:traffic"` and `pan:threat`)
- Parsed key fields: `src`, `dest`, `app`, `action`, `rule`, `threatid`, `category`
- Filtered for denied traffic, threat alerts, and rare outbound flows
- Mapped threat categories to MITRE ATT&CK tactics (e.g., Command and Control, Exfiltration)

### 📊 Dashboard Progress
- Panels created for:
  - Top denied applications and destination IPs
  - Threat signatures by severity and category
  - Traffic breakdown by rule, zone, and application
- Added dynamic filters for `action`, `app`, `threatid`, and `category`

### 📸 Screenshot
_No screenshot available for Day 6._

### 🧠 Observations
- Palo Alto logs offer deep visibility into both policy enforcement and threat detection
- Denied outbound traffic to rare IPs often signals staging or exfiltration attempts
- Threat signatures like `Suspicious TLS Beacon` and `Known C2 Traffic` are high-value indicators

### 🧪 Artifact Highlight: Threat Signature Detection

Detected outbound traffic flagged by Palo Alto as known C2 activity:

- **Event Time:** 08/24/2025 02:12:44 AM  
- **Source IP:** `192.168.56.101`  
- **Destination IP:** `185.62.189.11`  
- **Application:** `ssl`  
- **Action:** `alert`  
- **Threat ID:** `Suspicious TLS Beacon`  
- **Sourcetype:** `pan:threat`

This alert correlates with Suricata flow data from Day 5 and confirms multi-layer detection of the same outbound beacon.

---



