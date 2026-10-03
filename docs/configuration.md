# Splunk Configuration

This document describes the configuration used to collect Windows Event Logs with Splunk Universal Forwarder and forward them to Splunk Enterprise.

## 1. inputs.conf

The `inputs.conf` file defines which Windows Event Logs are collected by the Splunk Universal Forwarder.

```ini
[WinEventLog://Security]
disabled = 0
index = main

[WinEventLog://System]
disabled = 0
index = main
```

### Configuration Explanation

| Setting | Purpose |
|---|---|
| `WinEventLog://Security` | Collects Windows Security Event Logs |
| `WinEventLog://System` | Collects Windows System Event Logs |
| `disabled = 0` | Enables the input |
| `index = main` | Sends events to the main index |

## 2. outputs.conf

The `outputs.conf` file defines where the Universal Forwarder sends the collected events.

For the public repository, the server IP is replaced with a placeholder:

```ini
[tcpout]
defaultGroup = splunk_server

[tcpout:splunk_server]
server = <SPLUNK_SERVER_IP>:9997
```

The actual lab configuration uses the Splunk Enterprise server's private IP address with TCP port 9997.

### Configuration Explanation

| Setting | Purpose |
|---|---|
| `[tcpout]` | Defines the TCP forwarding configuration |
| `defaultGroup` | Specifies the default forwarding group |
| `[tcpout:splunk_server]` | Defines the forwarding destination |
| `server` | Specifies the Splunk server IP address and receiving port |

## 3. Log Forwarding

The completed forwarding process is:

```text
Windows Security/System Logs
          |
          v
Splunk Universal Forwarder
          |
          | TCP 9997
          v
Splunk Enterprise
          |
          v
       main index
```

## 4. Connectivity Testing

Connectivity between the Windows endpoint and Splunk server was tested using:

```powershell
Test-NetConnection <SPLUNK_SERVER_IP> -Port 9997
```

The test successfully established a TCP connection to port 9997.

## 5. Verification

After configuring the Universal Forwarder, Windows Security and System events were searched for in Splunk.

The presence of Windows Event Logs confirmed that:

- The Universal Forwarder was collecting events.
- The Windows endpoint could communicate with the Splunk server.
- Splunk Enterprise was receiving the forwarded data.
- The events were searchable in the main index.

## 6. Security Note

The actual private IP address of the Splunk server is not included in this public repository.

`<SPLUNK_SERVER_IP>` is used as a placeholder to avoid exposing the lab's private network configuration.
