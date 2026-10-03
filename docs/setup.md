\# Lab Setup



\## 1. Lab Overview



The lab consists of a Windows endpoint and an Ubuntu Server virtual machine running Splunk Enterprise.



The Windows endpoint uses Splunk Universal Forwarder to collect Windows Event Logs and forward them to the Splunk server.



\## 2. Lab Environment



| Component | Details |

|---|---|

| Windows Endpoint | Windows host machine |

| Splunk Forwarder | Splunk Universal Forwarder |

| Splunk Server | Ubuntu Server virtual machine |

| SIEM | Splunk Enterprise |

| Virtualization | Oracle VirtualBox |

| Forwarding Port | TCP 9997 |



\## 3. Network Architecture



The Ubuntu Server virtual machine uses a bridged network adapter so that it can communicate directly with the Windows host over the local network.



```text

Windows Host

&#x20;    |

&#x20;    | Local Network

&#x20;    |

&#x20;    v

Ubuntu Server VM

&#x20;    |

&#x20;    | TCP 9997

&#x20;    |

&#x20;    v

Splunk Enterprise

```



\## 4. Splunk Enterprise



Splunk Enterprise was installed on the Ubuntu Server virtual machine.



After installation, the Splunk service was started and verified to be running.



The Splunk web interface was accessed through:



```text

http://<SPLUNK\_SERVER\_IP>:8000

```



Splunk was configured to receive forwarded data on TCP port 9997.



\## 5. Splunk Receiving Configuration



The Splunk server was configured to listen for incoming data on:



```text

TCP 9997

```



The receiving configuration was verified using the Splunk command line.



\## 6. Splunk Universal Forwarder



Splunk Universal Forwarder was installed on the Windows endpoint.



The forwarder runs as the:



```text

SplunkForwarder

```



Windows service.



The service was configured to start automatically.



\## 7. outputs.conf



The Universal Forwarder was configured to send events to the Splunk server using the following destination:



```text

<SPLUNK\_SERVER\_IP>:9997

```



The configuration was tested from the Windows endpoint to verify that TCP port 9997 was reachable.



\## 8. inputs.conf



The Universal Forwarder was configured to collect Windows Event Logs.



The following logs were enabled:



\- Windows Security Event Log

\- Windows System Event Log



The events were assigned to the `main` index.



\## 9. Connectivity Testing



Connectivity between the Windows endpoint and Splunk server was tested using:



```powershell

Test-NetConnection <SPLUNK\_SERVER\_IP> -Port 9997

```



A successful TCP connection confirmed that the Windows endpoint could reach the Splunk receiving port.



\## 10. Log Verification



After configuring the Universal Forwarder, Windows Security and System events were searched for in Splunk.



The presence of Windows Event IDs confirmed that the forwarding pipeline was working.



Examples of observed events include:



\- 4624

\- 4634

\- 4647

\- 4648

\- 4672

\- 4738

\- 4798

\- 5379



\## 11. Result



The completed log collection pipeline is:



```text

Windows Event Logs

&#x20;       |

&#x20;       v

Splunk Universal Forwarder

&#x20;       |

&#x20;       | TCP 9997

&#x20;       v

Splunk Enterprise

&#x20;       |

&#x20;       v

Splunk Search \& Analysis

```



The lab successfully collects Windows Security and System events for security monitoring and investigation.

