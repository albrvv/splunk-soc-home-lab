\# Detection: Multiple Failed Logons



\## Objective



Detect repeated failed Windows logon attempts that may indicate password guessing or brute-force activity.



\## Windows Event ID



\- \*\*4625\*\*: An account failed to log on.



\## Splunk Search



```spl

index=main EventCode=4625

| stats count by Account\_Name, Source\_Network\_Address

| where count >= 5

| sort - count

```



\## Detection Logic



The search counts failed logon events by username and source network address.



If an account has 5 or more failed logon attempts from the same source, the activity can be investigated for possible password guessing.



\## Investigation



When the detection triggers, investigate:



\- Username targeted

\- Source IP address

\- Number of failed attempts

\- Time of the attempts

\- Whether successful logons occurred afterward

\- Whether the source IP belongs to a known device



\## Severity



Medium



\## Status



Detection documented. Testing and tuning required.

