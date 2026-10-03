\# Investigation: Successful Windows Logon



\## Objective



Investigate successful Windows logon activity and identify unusual authentication events.



\## Windows Event ID



\- \*\*4624\*\*: An account was successfully logged on.



\## Splunk Search



```spl

index=main EventCode=4624

| table \_time Account\_Name Logon\_Type Source\_Network\_Address Workstation\_Name

| sort - \_time

```



\## Investigation Steps



1\. Identify the account that logged on.

2\. Check the logon type.

3\. Check the source IP address.

4\. Check the workstation name.

5\. Review the time of the logon.

6\. Determine whether the activity is expected.



\## Related Events



The following events can provide additional context:



\- 4625: Failed logon

\- 4634: Logoff

\- 4647: User initiated logoff

\- 4672: Special privileges assigned to new logon

\- 4648: Logon using explicit credentials



\## Investigation Result



Successful logon events can be used to establish a timeline of user authentication activity and identify logons that require further investigation.



\## Status



Investigation procedure documented.

