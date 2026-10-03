\# SOC Dashboard



\## Objective



Create a basic SOC monitoring dashboard in Splunk to visualize Windows security activity.



\## Dashboard Panels



\### 1. Successful Logons



```spl

index=main EventCode=4624

| stats count by Account\_Name

| sort - count

```



Purpose: Monitor successful authentication activity.



\### 2. Failed Logons



```spl

index=main EventCode=4625

| stats count by Account\_Name

| sort - count

```



Purpose: Identify accounts with repeated failed logon attempts.



\### 3. Privileged Logons



```spl

index=main EventCode=4672

| stats count by Account\_Name

| sort - count

```



Purpose: Monitor logons associated with special privileges.



\### 4. Credential Manager Access



```spl

index=main EventCode=5379

| stats count by Account\_Name

| sort - count

```



Purpose: Monitor Credential Manager access activity.



\### 5. Windows Event Activity



```spl

index=main

| timechart count by EventCode

```



Purpose: Visualize Windows security event activity over time.



\## Monitoring Goals



The dashboard provides visibility into:



\- Authentication activity

\- Failed logons

\- Privileged activity

\- Credential access

\- Overall Windows security events



\## Status



Dashboard searches documented.



Splunk dashboard implementation pending.

