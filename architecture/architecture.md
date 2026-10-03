\# SOC Lab Architecture



\## Overview



The SOC home lab consists of a Windows endpoint that forwards Windows Event Logs to a Splunk Enterprise server running on an Ubuntu Server virtual machine.



\## Architecture Diagram



```text

┌──────────────────────┐

│   Windows Endpoint   │

│                      │

│ Windows Event Logs   │

└──────────┬───────────┘

&#x20;          │

&#x20;          │ Splunk Universal

&#x20;          │ Forwarder

&#x20;          │

&#x20;          │ TCP 9997

&#x20;          ▼

┌──────────────────────┐

│    Ubuntu Server     │

│      Virtual VM      │

│                      │

│  Splunk Enterprise   │

└──────────┬───────────┘

&#x20;          │

&#x20;          ▼

┌──────────────────────┐

│   Splunk Search \&    │

│      Analysis        │

│                      │

│ Detection /          │

│ Investigation        │

└──────────────────────┘

```



\## Network Configuration



The Ubuntu Server virtual machine uses a bridged network adapter.



This allows the Windows host and Ubuntu Server VM to communicate over the local network.



The Splunk receiving port is:



```text

TCP 9997

```



\## Data Flow



1\. Windows generates Security and System Event Logs.

2\. Splunk Universal Forwarder collects the events.

3\. The Forwarder sends the events to Splunk Enterprise over TCP 9997.

4\. Splunk indexes the events.

5\. The events are searched and analyzed using SPL.

6\. Detection and investigation searches are used to identify suspicious activity.



\## Components



| Component | Role |

|---|---|

| Windows Endpoint | Generates security and system logs |

| Splunk Universal Forwarder | Collects and forwards logs |

| Ubuntu Server VM | Hosts Splunk Enterprise |

| Splunk Enterprise | SIEM and log analysis |

| VirtualBox | Runs the Ubuntu Server VM |



\## Security Considerations



Private IP addresses are not included in this public documentation.



The lab is intended for local security monitoring and learning.

