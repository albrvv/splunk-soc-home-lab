# Splunk SOC Home Lab

> A personal Security Operations Center (SOC) home lab for practicing security monitoring, log collection, detection, and investigation with Splunk.

## Overview

This project documents a personal Security Operations Center (SOC) home lab built to practice security monitoring, log collection, detection, and investigation using Splunk.

The lab collects Windows Event Logs from a Windows endpoint using Splunk Universal Forwarder and forwards them to Splunk Enterprise running on an Ubuntu Server virtual machine.

## Lab Architecture

```text
┌──────────────────────┐
│   Windows Endpoint   │
│                      │
│  Windows Event Logs  │
└──────────┬───────────┘
           │
           │ Splunk Universal
           │ Forwarder
           │
           │ TCP 9997
           ▼
┌──────────────────────┐
│    Ubuntu Server     │
│      Virtual VM      │
│                      │
│  Splunk Enterprise   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│   Splunk Search &    │
│       Analysis       │
│                      │
│   Detection /        │
│   Investigation      │
└──────────────────────┘
```

The Ubuntu Server VM uses a bridged network adapter to communicate with the Windows endpoint over the local network.

## Technologies

- Splunk Enterprise
- Splunk Universal Forwarder
- Ubuntu Server
- Windows
- VirtualBox
- Windows Event Logs
- SPL (Search Processing Language)

## Objectives

- Collect Windows Security and System logs
- Configure centralized log ingestion
- Monitor authentication activity
- Analyze Windows security events
- Create security detection searches
- Investigate suspicious activity
- Build SOC monitoring dashboards
- Practice incident investigation

## Lab Components

| Component | Description |
|---|---|
| **Windows Endpoint** | Source of Windows Event Logs |
| **Splunk Universal Forwarder** | Collects and forwards Windows logs |
| **Ubuntu Server** | Hosts Splunk Enterprise |
| **Splunk Enterprise** | SIEM and log analysis platform |
| **VirtualBox** | Virtualization platform |

## Logs Collected

The lab currently collects:

- Windows Security Event Logs
- Windows System Event Logs

Examples of analyzed Windows Event IDs include:

| Event ID | Description |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Logoff |
| 4647 | User-initiated logoff |
| 4648 | Explicit credentials used |
| 4672 | Special privileges assigned |
| 4738 | User account changed |
| 4797 | User account existence/password query |
| 4798 | User local group membership enumerated |
| 4799 | Security-enabled local group enumerated |
| 4904 | Security event source registered |
| 4905 | Security event source unregistered |
| 4907 | Auditing settings changed |
| 5058 | Cryptographic key file operation |
| 5059 | Cryptographic key migration |
| 5061 | Cryptographic operation |
| 5379 | Credential Manager credentials read |
| 5382 | Credential Manager credentials backed up |

## Detection Engineering

Three Splunk detections were created and tested:

| Detection | Event ID | Description |
|---|---|---|
| **Multiple Failed Windows Logins** | 4625 | Detects 3 or more failed logins for the same account and source IP within 5 minutes |
| **Special Privileges Assigned** | 4672 | Summarizes accounts receiving special privileges |
| **Windows Account Changes** | 4738 | Provides account-change events for further investigation |

Detection documentation is available in:

```text
detections/
```

## Investigation

A Windows Credential Manager investigation was performed using Event ID 5379.

The investigation correlated:

- Event ID 5379
- Event ID 4624
- Event IDs 5058, 5059, and 5061
- Logon ID
- Process ID
- `svchost.exe`
- Connected Devices Platform User Service

The investigation demonstrated how multiple Windows security events and process/service information can be correlated to understand unusual activity.

Investigation documentation is available in:

```text
investigations/
```

## SOC Dashboard

A Splunk Dashboard Studio dashboard named **SOC Windows Security Monitoring** was created.

The dashboard contains eight monitoring panels:

1. Failed Login Attempts
2. Successful Logins
3. Failed Logins by Source IP
4. Security Events Over Time
5. Security Events by Event ID
6. Credential Manager Activity
7. Special Privileges Assigned
8. Account Changes

Dashboard documentation is available in:

```text
docs/dashboard.md
```

## Project Status

- [x] Splunk Enterprise installed
- [x] Splunk Universal Forwarder installed
- [x] Windows Security logs collected
- [x] Windows System logs collected
- [x] Splunk receiving data on TCP 9997
- [x] Windows events analyzed
- [x] Detection development completed
- [x] Detection documentation completed
- [x] Credential Manager investigation completed
- [x] Investigation documentation completed
- [x] SOC dashboard created
- [x] Dashboard documentation completed
- [x] Architecture documentation completed
- [x] Troubleshooting documentation completed
- [x] Public repository configuration sanitized

## Repository Structure

```text
splunk-soc-home-lab/
│
├── architecture/
├── configuration/
├── detections/
├── investigations/
├── screenshots/
├── docs/
└── README.md
```

## License Considerations

The lab uses the Splunk Free license.

Automated scheduled alerting was not implemented because of the license limitations.

The current project focuses on:

- Log collection
- SIEM searching
- Detection development
- Dashboard monitoring
- Event investigation

## Future Improvements

- Create additional detection rules
- Expand Windows event monitoring
- Improve SOC dashboards
- Add additional endpoints
- Add automated alerting if a suitable Splunk license is used
- Expand investigation workflows
