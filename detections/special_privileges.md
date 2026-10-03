# Detection: Special Privileges Assigned

## Description

Detects Windows Event ID 4672 and summarizes the accounts that received special privileges.

Event ID 4672 is generated when special privileges are assigned to a new logon. The event can provide useful context for monitoring privileged activity and investigating unexpected privileged logons.

## Windows Event ID

**Event ID:** 4672  
**Event Name:** Special privileges assigned to new logon

## SPL Query

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4672
| stats count by Account_Name
| sort - count
```

## Detection Logic

The search:

- Searches Windows Security logs.
- Filters for Event ID 4672.
- Groups events by account.
- Counts the number of Event 4672 events for each account.
- Sorts the accounts by event count.

This is a generic monitoring search rather than an account-specific detection.

## Lab Results

During testing, the search returned **1,233** Event ID 4672 events for the selected period:

**September 3, 2026 to October 3, 2026**

| Account | Event Count |
|---|---:|
| SYSTEM | 1,151 |
| albraa.haitham117@hotmail.com | 46 |
| DWM-1 | 10 |
| SplunkForwarder | 6 |
| LOCAL SERVICE | 5 |
| NETWORK SERVICE | 5 |
| DWM-2 | 4 |
| DWM-3 | 4 |
| DWM-4 | 2 |

## Investigation

Event ID 4672 does not automatically indicate malicious activity.

Special privileges can be assigned during legitimate administrative or system activity. High-volume activity from accounts such as SYSTEM or Windows service accounts should therefore be interpreted in context.

The results can be used as a starting point for further investigation by reviewing:

- Account
- Logon ID
- Logon type
- Source address
- Process information
- Related authentication events
- Timing and frequency
- Expected system or service activity

## Why This Detection Matters

Unexpected privileged logons can be relevant to SOC monitoring because attackers may attempt to obtain or use accounts with elevated privileges.

Monitoring Event ID 4672 provides visibility into privileged logon activity and can help analysts identify events that require additional investigation.

## Limitations

Event ID 4672 alone does not establish that an account or activity is malicious.

The search currently summarizes all observed accounts rather than applying a fixed privileged-account allowlist or baseline.

In a production environment, the results could be enhanced with account baselines, known service accounts, logon context, and correlation with other Windows Security events.

## Status

Tested and documented
