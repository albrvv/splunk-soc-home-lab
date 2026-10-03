# Detection: Special Privileges Assigned

## Description

Detects Windows Event ID 4672 for the monitored user account.

Event ID 4672 is generated when special privileges are assigned to a new logon. These privileges can provide elevated capabilities and may require investigation when unexpected.

## Windows Event ID

**Event ID:** 4672  
**Event Name:** Special privileges assigned to new logon

## SPL Query

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4672
| search Account_Name="albraa.haitham117@hotmail.com"
| stats count by Account_Name
```

## Detection Logic

The search:

- Searches Windows Security logs.
- Filters for Event ID 4672.
- Monitors the specific lab user account.
- Counts the number of matching events.

## Lab Results

During testing, the monitored account:

**Account:** albraa.haitham117@hotmail.com

generated 10 Event ID 4672 events.

## Investigation

Event ID 4672 does not automatically indicate malicious activity.

Special privileges can be assigned during legitimate administrative or system activity.

The events were therefore treated as monitoring and investigation points rather than confirmed security incidents.

## Why This Detection Matters

Special privileges can provide access to sensitive system operations.

Monitoring Event ID 4672 can help identify unexpected privileged logons and provide additional context during investigations.

This event can be particularly useful when correlated with other Windows Security events such as:

- Event ID 4624: Successful logon
- Event ID 4625: Failed logon
- Event ID 4648: Explicit credentials used
- Event ID 4738: User account changed

## Limitations

The detection monitors a specific account in this lab environment.

In a production environment, the detection could be expanded to monitor privileged accounts or investigate unusual privilege assignments based on baseline behavior.

## Status

Tested and documented
