# Splunk-Installation-and-Monitoring
A beginner-friendly Splunk setup guide with detailed steps for log forwarding, SSH/ICMP attack detection, and alert creation using Splunk Enterprise and Universal Forwarder.


 
# 📊 Splunk Log Monitoring & Attack Detection

[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Linux-blue.svg)]()
[![Splunk](https://img.shields.io/badge/splunk-enterprise-brightgreen)](https://www.splunk.com/)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)](CONTRIBUTING.md)

---

## 🚀 Overview

A complete Splunk setup tutorial for collecting, forwarding, and analyzing logs from Linux systems to a Windows-based Splunk Enterprise server. This guide includes attack detection for **SSH brute force**, **ICMP flood**, and **unauthorized user creation**, with steps to configure alerts and dashboards.

---

## 📦 Features

- ✅ Splunk Enterprise setup on Windows
- ✅ Universal Forwarder configuration on Linux
- ✅ Port forwarding and firewall setup (Port `9997`)
- ✅ Real-time log monitoring (e.g., syslog, auth.log)
- ✅ Attack detection:
  - 🔓 SSH brute-force attempts
  - 📡 ICMP ping floods
  - 👥 New user creation
- ✅ SPL search queries and alert configuration
- ✅ Scheduled alerts via cron

---

## 🧰 Prerequisites

- Splunk Enterprise installed (on Windows)
- Splunk Universal Forwarder (on Linux)
- Admin access on both systems
- Basic networking & Linux CLI familiarity

---

## ⚙️ Setup Guide

### 1. Install & Launch Splunk Enterprise

- Download from: [splunk.com](https://www.splunk.com)
- Access UI: `http://localhost:8000`

### 2. Enable Port Receiving on Splunk

- Navigate: `Settings → Forwarding and Receiving → Configure Receiving`
- Add port: `9997`

### 3. Install & Configure Universal Forwarder on Linux

```bash
# Connect to Splunk Indexer
/opt/splunkforwarder/bin/splunk add forward-server <Windows_IP>:9997 -auth admin:<password>

# Monitor log directories
/opt/splunkforwarder/bin/splunk add monitor /var/log -auth admin:<password>
/opt/splunkforwarder/bin/splunk restart
```

### 4. Verify Logs in Splunk UI

```spl
index=_internal host=<Linux_Hostname>
index=main source="/var/log/syslog"
```

---

## 🛡️ Attack Detection & SPL Queries

### 🔐 SSH Brute Force

- **Log File:** `/var/log/auth.log`

```bash
./splunk add monitor /var/log/auth.log
```

```spl
index=ssh_logs "Failed password"
```

---

### 🌐 ICMP Flood Detection

```spl
index=* ("ICMP Echo" OR icmp OR "type=8")
```

- Save as alert:
  - Title: `ICMP Ping Alert`
  - Trigger: `When results > 0`
  - Action: `Email` or `Webhook`

---

### 👤 Unauthorized User Creation

```spl
index=main source="/var/log/auth.log" "useradd" "new user"
```

---

## ⏱️ Cron Scheduling for Alerts

Use cron-style scheduling for recurring alert checks.

```spl
index=* ("ICMP Echo" OR icmp OR "type=8")
```

- `Save As → Alert → Schedule: Cron`
- Action: Email/Webhook

---

## 🧪 Test & Troubleshoot

- 🛑 Check Windows Firewall: Allow TCP port 9997
- 🌐 Verify network reachability: `ping <Windows_IP>`
- 🖥️ Use Bridged Adapter in VirtualBox if needed

---

## 📂 Project Structure

```
📁 splunk-docs/
├── README.md
├── Splunk Documentation.docx
├── setup-scripts/
├── alerts/
└── queries/
```

---

## 📚 Resources

- [Splunk Docs](https://docs.splunk.com)
- [Universal Forwarder Setup](https://docs.splunk.com/Documentation/Forwarder/latest/Forwarder/Installtheuniversalforwarder)

---

## 🤝 Contributing

Contributions, ideas, and bug reports are welcome!
Feel free to open a Pull Request or Issue.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 🙌 Author

Made with ❤️ by Anzal Ps  
Connect on [LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com/)
