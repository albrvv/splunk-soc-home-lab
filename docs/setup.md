# Lab Setup

> Setup documentation for a personal SOC home lab: a Windows endpoint forwards Windows Event Logs to Splunk Enterprise running on an Ubuntu Server virtual machine.

## Overview

This document describes the setup of the Splunk SOC home lab.

The lab uses a Windows endpoint to generate Windows Event Logs and an Ubuntu Server virtual machine running Splunk Enterprise to collect, index, search, and analyze the logs.

## Lab Architecture

The basic data flow is:

```text
Windows Endpoint
      |
      | Windows Event Logs
      v
Splunk Universal Forwarder
      |
      | TCP 9997
      v
Ubuntu Server VM
      |
      v
Splunk Enterprise
      |
      v
Search / Dashboard / Detection / Investigation
```

## Components

| Component | Role |
|---|---|
| **Windows Endpoint** | Generates Windows Security and System events |
| **Splunk Universal Forwarder** | Collects and forwards Windows Event Logs |
| **Ubuntu Server** | Hosts Splunk Enterprise |
| **Splunk Enterprise** | Receives, indexes, searches, and analyzes logs |
| **VirtualBox** | Runs the Ubuntu Server virtual machine |

## 1. Create the Ubuntu Server VM

An Ubuntu Server virtual machine was created using VirtualBox.

The VM is used as the Splunk Enterprise server.

The VM uses a Bridged Adapter so that it can communicate with the Windows host over the local network.

## 2. Install Splunk Enterprise

Splunk Enterprise was installed on the Ubuntu Server VM.

| Setting | Value |
|---|---|
| Installation directory | `/opt/splunk` |
| Splunk Web port | `8000` |
| Forwarder receiving port | `9997` |

## 3. Configure Splunk Receiving

Splunk Enterprise was configured to receive forwarded data on TCP port 9997.

The receiving configuration allows the Windows Universal Forwarder to send Windows Event Logs to the Splunk server.

## 4. Install Splunk Universal Forwarder

The Splunk Universal Forwarder was installed on the Windows endpoint.

The Windows service is:

```text
SplunkForwarder
```

The service is configured to start automatically with Windows.

## 5. Configure Windows Event Log Collection

The Universal Forwarder was configured to collect Windows Security and System Event Logs.

The configuration uses:

```ini
[WinEventLog://Security]
disabled = 0
index = main

[WinEventLog://System]
disabled = 0
index = main
```

The logs are sent to the Splunk `main` index.

## 6. Configure Log Forwarding

The Universal Forwarder was configured to send data to the Splunk Enterprise server on TCP port 9997.

A sanitized version of the configuration is stored in:

```text
configuration/outputs.conf
```

The public configuration uses a placeholder instead of exposing the private IP address of the lab server.

Example:

```ini
[tcpout]
defaultGroup = splunk_server

[tcpout:splunk_server]
server = <SPLUNK_SERVER_IP>:9997
```

The actual lab configuration uses the private IP address of the Splunk server.

## 7. Verify Network Connectivity

Network connectivity between the Windows endpoint and Ubuntu Server was tested using TCP port 9997.

The Windows endpoint successfully connected to the Splunk receiving port.

This confirmed that the Universal Forwarder could communicate with Splunk Enterprise.

## 8. Verify Log Ingestion

After configuring the Universal Forwarder, the Windows Security and System events were searched in Splunk.

Example search:

```spl
index=main
```

Security events can be searched using:

```spl
index=main sourcetype="WinEventLog:Security"
```

System events can be searched using:

```spl
index=main sourcetype="WinEventLog:System"
```

Successful ingestion was confirmed by the presence of Windows events in Splunk.

## 9. Security Event Monitoring

The lab focuses primarily on Windows Security Event Logs.

Important events observed during the project include:

| Event ID | Description |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4648 | Explicit credentials used |
| 4672 | Special privileges assigned |
| 4738 | User account changed |
| 4798 | User local group membership enumerated |
| 5379 | Credential Manager credentials read |

Additional event codes are documented in:

```text
docs/event-codes.md
```

## 10. Detection Development

Three Splunk detections were created and tested:

1. Multiple Failed Windows Logins
2. Special Privileges Assigned
3. Windows Account Changes

The detection searches are documented in:

```text
detections/
```

## 11. Dashboard

A Splunk Dashboard Studio dashboard named **SOC Windows Security Monitoring** was created.

The dashboard contains eight monitoring panels covering:

- Failed login attempts
- Successful logins
- Failed logins by source IP
- Security events over time
- Security events by Event ID
- Credential Manager activity
- Special privileges assigned
- Account changes

Dashboard documentation is available in:

```text
docs/dashboard.md
```

## 12. Investigation

The lab was also used to investigate Windows security events.

One investigation focused on high-volume Event ID 5379 Credential Manager activity.

The investigation correlated the events with:

- Logon ID
- Event ID 4624
- Cryptographic events
- Process ID
- `svchost.exe`
- Connected Devices Platform User Service

This demonstrated the use of event correlation and Windows process/service information during a SOC investigation.

## 13. License Considerations

The lab uses the Splunk Free license.

Because of the license limitations, automated scheduled alerting was not implemented.

The project currently focuses on:

- Log collection
- SIEM searching
- Detection development
- Dashboard monitoring
- Event investigation

Automated alerting can be added in a future version if the lab is moved to a license that supports the required alerting features.

## 14. Public Repository Considerations

Private network information is not included in the public repository.

Configuration examples use placeholders such as:

```text
<SPLUNK_SERVER_IP>
```

This allows the configuration to be documented without exposing private network addressing.

## Status

The core lab setup is complete and operational.

The project currently includes:

- Windows Event Log collection
- Splunk Enterprise
- Universal Forwarder
- TCP 9997 forwarding
- Windows Security monitoring
- Three tested detections
- SOC monitoring dashboard
- Event documentation
- Investigation documentation
