# Mini SOC Lab — Setup Guide

## 1. Lab Architecture

The lab uses three main components:

- Windows 11 — generates Windows Security events.
- Ubuntu on WSL 2 — generates Linux authentication and sudo logs.
- Splunk Enterprise — centralizes and analyzes security logs.

Splunk Universal Forwarder runs on Windows and forwards Windows Security events to Splunk Enterprise.

## 2. Splunk Enterprise Setup

- Installed Splunk Enterprise on Ubuntu WSL 2.
- Started Splunk as the dedicated `splunk` user.
- Enabled the receiving port `9997`.
- Accessed Splunk Web at `http://localhost:8000`.

## 3. Windows Log Collection

Installed Splunk Universal Forwarder on Windows.

Configured `outputs.conf` to forward events to the Splunk Enterprise receiver.

Configured `inputs.conf` to monitor Windows Security logs:

```ini
[WinEventLog://Security]
disabled = 0
index = main
renderXml = 1
```

Restarted the Universal Forwarder service.

Verified incoming events in Splunk using the `XmlWinEventLog:Security` sourcetype.

## 4. Linux Log Collection

Configured Splunk Enterprise to monitor:

`/var/log/auth.log`

Added the Splunk service account to the `adm` group to allow log access.

Configured the input:

```ini
[monitor:///var/log/auth.log]
disabled = 0
index = main
sourcetype = linux_secure
```

Restarted Splunk and verified incoming Linux authentication events.

## 5. Verification

Confirmed:
- Windows Security events were indexed.
- Event IDs 4624 and 4625 were searchable.
- Linux authentication logs were indexed.
- Sudo command activity was searchable.
- Dashboard visualizations displayed Windows and Linux activity.

## 6. Environment

- Windows 11
- Ubuntu 26.04 LTS on WSL 2
- Splunk Enterprise 10.6.1.1
- Splunk Universal Forwarder 10.4.3

## Note

This documentation describes a local educational lab. Network addresses and credentials are intentionally not included.
