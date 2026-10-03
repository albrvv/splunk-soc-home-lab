\# Troubleshooting



\## Issue: Splunk Server IP Address Changed



\### Problem



The Ubuntu Server virtual machine was using a DHCP-assigned IP address.



After the VM's IP address changed, the Splunk Universal Forwarder was still configured to send data to the previous IP address.



As a result, the Windows endpoint could no longer forward events to Splunk.



\### Symptoms



\- Windows Event Logs stopped appearing in Splunk.

\- The Universal Forwarder service was running.

\- The forwarding configuration still contained the previous Splunk server IP address.

\- TCP connectivity to the old IP address failed.



\### Investigation



The IP address of the Ubuntu Server was checked using:



```bash

ip addr show enp0s3

```



The current Splunk server IP address was identified.



The Universal Forwarder's `outputs.conf` was then checked to determine which destination address was configured.



\### Solution



The Splunk server address in `outputs.conf` was updated:



```ini

\[tcpout]

defaultGroup = splunk\_server



\[tcpout:splunk\_server]

server = <SPLUNK\_SERVER\_IP>:9997

```



The Windows endpoint was then tested using:



```powershell

Test-NetConnection <SPLUNK\_SERVER\_IP> -Port 9997

```



The test successfully established a TCP connection.



The Splunk Universal Forwarder was then restarted and log forwarding was verified in Splunk.



\### Result



Windows Security and System events began appearing in Splunk again.



\### Lesson Learned



Using DHCP can cause the Splunk server's IP address to change.



For a more stable lab environment, a static IP address or DHCP reservation can be configured for the Splunk server.

