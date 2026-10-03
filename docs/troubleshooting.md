# Troubleshooting

This document records troubleshooting performed during the setup and operation of the Splunk SOC home lab.

## Issue: Splunk Server IP Address Changed

### Problem

The Ubuntu Server virtual machine uses a DHCP-assigned IP address.

If the VM receives a different IP address, the Windows Universal Forwarder may still be configured to send events to the previous IP address.

As a result, Windows events may stop appearing in Splunk.

### Symptoms

- Windows Event Logs stop appearing in Splunk.
- The Universal Forwarder service is still running.
- `outputs.conf` contains the previous Splunk server IP address.
- TCP connectivity to the configured Splunk server address fails.

### Investigation

The current IP address of the Ubuntu Server was checked using:

```bash
ip addr show enp0s3
```

The Universal Forwarder's forwarding configuration was then checked to identify the configured destination:

```text
C:\Program Files\SplunkUniversalForwarder\etc\system\local\outputs.conf
```

### Solution

The Splunk server address in `outputs.conf` was updated to the current VM address.

For the public repository, the configuration is represented using a placeholder:

```ini
[tcpout]
defaultGroup = splunk_server

[tcpout:splunk_server]
server = <SPLUNK_SERVER_IP>:9997
```

Connectivity was then tested from the Windows endpoint:

```powershell
Test-NetConnection <SPLUNK_SERVER_IP> -Port 9997
```

The test successfully established a TCP connection.

The Universal Forwarder was restarted and Windows events were verified in Splunk.

### Result

Windows Security and System events began appearing in Splunk again.

### Lesson Learned

The lab currently uses DHCP for the Splunk server.

Because the Universal Forwarder depends on the Splunk server's IP address, a change in the VM's IP address can interrupt log forwarding.

A static IP address or DHCP reservation would provide a more stable configuration for future improvements.

## General Troubleshooting Checks

If Windows events are not appearing in Splunk, check the following:

### 1. Check the Splunk Server IP

On Ubuntu:

```bash
ip addr show enp0s3
```

### 2. Check Splunk Receiving Port

On the Splunk server, verify that TCP port `9997` is configured for receiving data.

### 3. Test Network Connectivity

From Windows:

```powershell
Test-NetConnection <SPLUNK_SERVER_IP> -Port 9997
```

The expected result is:

```text
TcpTestSucceeded : True
```

### 4. Check the Universal Forwarder

Verify that the Windows service is running:

```powershell
Get-Service SplunkForwarder
```

### 5. Check Forwarding Configuration

Verify that `outputs.conf` points to the current Splunk server address and port `9997`.

### 6. Check Splunk Searches

In Splunk, search:

```spl
index=main
```

For Windows Security events:

```spl
index=main sourcetype="WinEventLog:Security"
```

For Windows System events:

```spl
index=main sourcetype="WinEventLog:System"
```

## Current Lab Status

- Windows Universal Forwarder: Working
- Splunk Enterprise: Working
- TCP forwarding on port `9997`: Working
- Windows Security logs: Working
- Windows System logs: Working
- DHCP reservation: Not currently configured
- Automated alerting: Not available with the current Splunk Free license
