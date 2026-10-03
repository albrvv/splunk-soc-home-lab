\# Splunk SOC Home Lab



\## Overview



This project documents a personal Security Operations Center (SOC) home lab built to practice security monitoring, log collection, detection, and investigation using Splunk.



The lab collects Windows Event Logs from a Windows endpoint using Splunk Universal Forwarder and forwards them to Splunk Enterprise running on an Ubuntu Server virtual machine.



\## Lab Architecture



```text

Windows Endpoint

x.x.x.x

&#x20;     |

&#x20;     | Splunk Universal Forwarder

&#x20;     |

&#x20;     v

Ubuntu Server

y.y.y.y

&#x20;     |

&#x20;     | TCP 9997

&#x20;     |

&#x20;     v

Splunk Enterprise

&#x20;     |

&#x20;     v

Windows Event Logs

(Security + System)

```



\## Technologies



\- Splunk Enterprise

\- Splunk Universal Forwarder

\- Ubuntu Server

\- Windows

\- VirtualBox

\- Windows Event Logs

\- SPL (Search Processing Language)



\## Objectives



\- Collect Windows security and system logs

\- Configure centralized log ingestion

\- Monitor authentication activity

\- Analyze Windows security events

\- Create security detection searches

\- Investigate suspicious activity

\- Build SOC monitoring dashboards

\- Practice incident investigation



\## Lab Components



| Component | Description |

|---|---|

| Windows Endpoint | Source of Windows Event Logs |

| Splunk Universal Forwarder | Collects and forwards Windows logs |

| Ubuntu Server | Hosts Splunk Enterprise |

| Splunk Enterprise | SIEM and log analysis platform |

| VirtualBox | Virtualization platform |



\## Logs Collected



The lab currently collects:



\- Windows Security Event Logs

\- Windows System Event Logs



Examples of analyzed Windows Event IDs include:



| Event ID | Description |

|---|---|

| 4624 | Successful logon |

| 4634 | Logoff |

| 4647 | User initiated logoff |

| 4648 | Logon using explicit credentials |

| 4672 | Special privileges assigned to new logon |

| 4738 | User account changed |

| 4798 | User's local group membership enumerated |

| 5379 | Credential Manager credentials read |



\## Skills Demonstrated



\- SIEM configuration

\- Windows Event Log analysis

\- Security monitoring

\- SPL

\- Detection engineering fundamentals

\- Authentication monitoring

\- Incident investigation

\- Network troubleshooting

\- Log collection and forwarding



\## Project Status



\- \[x] Splunk Enterprise installed

\- \[x] Splunk Universal Forwarder installed

\- \[x] Windows Security logs collected

\- \[x] Windows System logs collected

\- \[x] Splunk receiving data on TCP 9997

\- \[x] Windows events analyzed

\- \[ ] Detection documentation

\- \[ ] Investigation documentation

\- \[ ] Dashboard documentation

\- \[ ] Architecture diagram

\- \[ ] Troubleshooting documentation



\## Repository Structure



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



\## Future Improvements



\- Create additional detection rules

\- Configure automated alerts

\- Expand Windows event monitoring

\- Improve SOC dashboards

\- Add additional endpoints

\- Document investigation workflows

