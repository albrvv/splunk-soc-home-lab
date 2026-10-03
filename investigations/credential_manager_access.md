\# Investigation: Credential Manager Access



\## Objective



Investigate Windows Credential Manager access events and determine whether the activity is expected.



\## Windows Event ID



\- \*\*5379\*\*: Credential Manager credentials were read.



\## Splunk Search



```spl

index=main EventCode=5379

| table \_time Account\_Name ComputerName Process\_Name

| sort - \_time

```



\## Investigation Steps



1\. Identify the account associated with the event.

2\. Review the timestamp.

3\. Identify the process responsible for the activity.

4\. Check whether the activity occurred during normal user activity.

5\. Review related authentication events.

6\. Look for unusual or repeated credential access.



\## Related Events



Useful events for additional context include:



\- 4624: Successful logon

\- 4648: Logon using explicit credentials

\- 4672: Special privileges assigned to new logon

\- 4688: A new process has been created



\## Investigation Result



Credential Manager access is not automatically malicious. The event should be correlated with the user, process, time, and other security events to determine whether the activity is expected.



\## Status



Investigation documented. Further analysis can be performed using collected lab data.

