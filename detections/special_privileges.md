\# Detection: Special Privileges Assigned



\## Objective



Detect logons where special privileges are assigned to a user account.



\## Windows Event ID



\- \*\*4672\*\*: Special privileges assigned to new logon.



\## Splunk Search



```spl

index=main EventCode=4672

| stats count by Account\_Name, Logon\_ID

| sort - count

```



\## Detection Logic



Event ID 4672 is generated when a new logon receives special privileges.



This can be normal for administrators and system accounts, but unexpected occurrences should be investigated.



\## Investigation



When the detection triggers, investigate:



\- Account name

\- Logon ID

\- Time of the event

\- Logon type

\- Source IP address

\- Whether the account is expected to have administrative privileges

\- Related successful logon events (4624)



\## Severity



Medium



\## Status



Detection documented. Testing and tuning required.

