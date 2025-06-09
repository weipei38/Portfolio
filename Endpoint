# Rapid Endpoint Investigation – Training Takeaways  

## Overview  
Throughout this training, I completed four labs focused on **rapid forensic artifact collection, endpoint investigation, data parsing, output analysis, and Linux forensic analysis** using tools like **Velociraptor** and **KAPE**. These exercises reinforced my understanding of **triage workflows, persistence mechanisms, and forensic artifact analysis**, preparing me for real-world cybersecurity investigations.  

## Key Learnings & Applications  

### 1. Streamlining Artifact Collection  
- Used **Velociraptor Offline Collector** to gather forensic data from Windows and Linux endpoints.  
- Configured **KAPE triage collections** to extract logs, registry hives, and system artifacts efficiently.  
- Developed structured workflows for **artifact selection**, balancing speed and thoroughness in investigations.  

### 2. Investigating Network & Process Activity  
- Analyzed **Windows.Network.NetstatEnriched** to identify active connections and potential command-and-control (C2) activity.  
- Examined **Windows.System.Pslist** to detect unauthorized process execution and possible malware infection.  
- Reviewed **Windows.System.DNSCache** to track domain resolutions linked to phishing or cyberattacks.  

### 3. Persistence & System Integrity Checks  
- Leveraged **Windows.Sysinternals.Autoruns** to detect startup programs used for persistence.  
- Examined **Windows.System.Services** to ensure system services weren’t exploited by attackers.  
- Explored **registry artifacts** to uncover traces of privilege escalation techniques.  

### 4. Parsing Triage Data for Investigation (Lab 2.0)  
- **Decompressed and staged forensic data** from Lab 1.0 for structured analysis.  
- **Used PowerShell scripts** (`expand-archive-triage-data.ps1`) to extract triage collections efficiently.  
- **Refined KAPE processing** using pre-configured scripts to parse event logs, browser artifacts, and MFT data.  
- **Filtered EVTX event logs** for key indicators (e.g., failed logins, account changes, audit log clearing).  
- **Examined executable file listings** from the Master File Table (MFT) to detect suspicious file creations.  

### 5. Windows Output Analysis (Lab 3.0)  
- **Applied rapid triage workflow** to analyze parsed forensic data and derive actionable intelligence.  
- **Filtered out low-fidelity artifacts** to focus on high-value indicators of compromise (IOCs).  
- **Investigated running processes** using **PSList**, identifying suspicious executables and their SHA1 hashes.  
- **Reviewed network sockets** to detect malicious communications, including destination IPs and ports.  
- **Correlated findings** across multiple artifacts (event logs, process lists, network connections) to determine attack extent.  
- **Documented findings** using structured notes, ensuring clear investigative records for future reference.  

### 6. Linux Output Analysis (Lab 4.0)  
- **Investigated Linux processes** using `processes-axwwSo.txt`, filtering for non-root users to detect anomalies.  
- **Identified suspicious executables** (`.system3d` and `Update`) and mapped their SHA1 hashes for reputation analysis.  
- **Analyzed network sockets** (`ss-anepo.txt`) to detect potential command-and-control (C2) activity.  
- **Correlated findings** across process listings, network connections, and disk artifacts to determine attack extent.  
- **Reviewed disk timeline artifacts** (`collection-full-timeline.csv`) to track suspicious file modifications.  
- **Examined cron jobs** (`collection-cron-tab-list.txt`) for persistence mechanisms used by attackers.  

## Challenges Faced & Lessons Learned  
✔ **Balancing speed with accuracy** – The goal was rapid triage, but avoiding unnecessary data collection was key.  
✔ **Understanding artifact correlations** – DNS queries and netstat logs had to be examined together for deeper insights.  
✔ **Handling noisy datasets** – Filtering relevant logs without losing critical forensic details took careful planning.  
✔ **Troubleshooting script execution** – Ensuring **correct PowerShell versions and permissions** was essential for smooth artifact parsing.  
✔ **Refining investigative documentation** – Learned to streamline documentation without sacrificing detail, making reports more actionable.  
✔ **Adapting Windows workflows to Linux** – Linux forensic analysis required different approaches, particularly in process filtering and persistence detection.  

## Impact on My Cybersecurity Skillset  
🔹 **Improved forensic investigation speed** – Faster detection of anomalies and suspicious patterns using structured workflows.  
🔹 **Enhanced artifact-driven analysis** – Focused on **relevant system logs** first, instead of collecting excessive raw data.  
🔹 **Refined forensic tool proficiency** – Developed hands-on expertise with **Velociraptor, KAPE, PowerShell, and Linux forensic analysis**.  
🔹 **Strengthened endpoint triage methods** – Built processes for **artifact prioritization**, improving detection and response times.  
🔹 **Sharpened analytical skills** – Learned to **ask the right questions** to uncover deeper attack details.  

---

This markdown version is ready for **your GitHub portfolio**! 🚀 Would you like me to add any links to relevant repositories or references?  
