# windows-linux-mini-soc-lab
Hands-on Mini SOC lab using Splunk to monitor Windows Security and Linux authentication logs

# Windows & Linux Mini SOC Lab

A hands-on Security Operations Center (SOC) monitoring lab built using Splunk Enterprise, Splunk Universal Forwarder, Windows 11, and Ubuntu on WSL 2.

## Project Overview

This project demonstrates how security logs from Windows and Linux systems can be collected, centralized, searched, and visualized using Splunk.

The lab simulates basic SOC analyst activities, including authentication monitoring, failed-login investigation, privileged command monitoring, and security dashboard creation.

## Architecture

- **Windows 11:** Source of Windows Security events.
- **Splunk Universal Forwarder:** Collects and forwards Windows Security logs.
- **Ubuntu (WSL 2):** Source of Linux authentication logs.
- **Splunk Enterprise:** Receives, indexes, searches, and visualizes security events.

## Tools & Technologies

- Splunk Enterprise
- Splunk Universal Forwarder
- Windows 11 Event Viewer
- Ubuntu Linux (WSL 2)
- SPL (Search Processing Language)
- PowerShell
- Linux command line

## Implemented Features

### 1. Windows Security Monitoring
- Collected Windows Security events using Universal Forwarder.
- Monitored successful logins (Event ID 4624).
- Investigated failed login attempts (Event ID 4625).
- Analyzed authentication activity by username.
- Built failed-login detection searches.

### 2. Linux Log Monitoring
- Configured Splunk to monitor `/var/log/auth.log`.
- Collected Linux authentication and sudo activity.
- Investigated privileged command execution.
- Monitored sudo activity over time.

### 3. Security Dashboard

Created a Splunk Dashboard Studio dashboard containing:

- Total successful logins
- Total failed logins
- Failed logins by username
- Failed logins over time
- Linux sudo activity over time
- Top Linux privileged commands

## Example SPL Searches

### Failed Windows Logins

```spl
index=main sourcetype="XmlWinEventLog:Security"
| rex "<EventID>(?<event_id>\d+)</EventID>"
| search event_id=4625
| rex field=_raw "<Data Name='TargetUserName'>(?<username>[^<]+)</Data>"
| stats count as failed_attempts by username
| sort - failed_attempts
```

### Successful Windows Logins

```spl
index=main sourcetype="XmlWinEventLog:Security"
| rex "<EventID>(?<event_id>\d+)</EventID>"
| search event_id=4624
| stats count as successful_logins
```

### Linux Sudo Commands

```spl
index=main sourcetype=linux_secure "COMMAND="
| rex field=_raw "COMMAND=(?<command>[^;]+)"
| stats count by command
| sort - count
| head 10
```

## Key Learning Outcomes

- Understanding centralized security log collection.
- Working with Windows authentication event IDs.
- Investigating failed and successful login activity.
- Searching and analyzing Linux authentication logs.
- Writing SPL queries for security monitoring.
- Building security monitoring dashboards.
- Understanding basic SOC investigation workflows.

## Environment

- Windows 11
- Ubuntu 26.04 LTS (WSL 2)
- Splunk Enterprise 10.6.1.1
- Splunk Universal Forwarder 10.4.3

## Disclaimer

This is an educational lab environment. Detection thresholds and monitoring rules are illustrative and should not be treated as production-ready security policies.
