# Detection: Multiple Failed Windows Logins

## Description

Detects multiple failed Windows login attempts for the same account and source IP within a 5-minute window.

This detection can help identify repeated authentication failures that may require investigation.

## Windows Event ID

**Event ID:** 4625  
**Event Name:** An account failed to log on

## SPL Query

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4625
| bin _time span=5m
| stats count as failed_logins by _time, Account_Name, Source_Network_Address
| where failed_logins >= 3
| sort - failed_logins
```

## Detection Logic

The search:

- Searches Windows Security logs.
- Filters for Event ID 4625.
- Groups events into 5-minute time windows.
- Groups failed logins by account and source IP.
- Detects when there are 3 or more failed attempts.
- Sorts the results by the number of failed attempts.

## Lab Results

During testing, the detection identified multiple failed login attempts from:

**Source IP:** 192.168.1.24

The activity included attempts involving the disabled Guest account.

A total of 4 failed login events were observed within the 5-minute window.

## Investigation

The failed login events were reviewed using additional Windows Security event fields.

The events showed:

- **Logon Type:** 3
- **Source Network Address:** 192.168.1.24
- **Failure Reason:** Account currently disabled
- **Authentication Package:** NTLM

The source IP belonged to the Windows endpoint used in the lab.

Based on the available evidence, the activity appeared consistent with local lab activity involving the disabled Guest account.

## Why This Detection Matters

Repeated failed authentication attempts can be relevant to security monitoring because they may indicate:

- Brute-force attempts
- Repeated incorrect credentials
- Misconfigured services
- Unauthorized authentication attempts

Additional investigation is required to determine whether the activity is legitimate or suspicious.

## Limitations

A threshold of 3 failed logins was selected because the initial 5-attempt threshold did not produce results in the available lab data.

The threshold may be adjusted depending on the environment and normal authentication behavior.

## Status

Tested and documented
