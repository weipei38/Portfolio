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

---

