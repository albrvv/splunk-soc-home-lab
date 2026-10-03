# SOC Windows Security Monitoring Dashboard

## Overview

The **SOC Windows Security Monitoring** dashboard was created in Splunk to provide a centralized view of Windows security activity collected from the lab endpoint.

The dashboard focuses on authentication activity, security events, credential-related activity, privileged access, and account changes.

## Dashboard Panels

The dashboard contains eight panels.

### 1. Failed Login Attempts

Displays multiple failed Windows login attempts detected using Event ID 4625.

**SPL:**

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4625
| bin _time span=5m
| stats count as failed_logins by _time, Account_Name, Source_Network_Address
| where failed_logins >= 3
| sort - failed_logins
```

**Purpose:** Identify repeated failed authentication attempts that may require investigation.

### 2. Successful Logins

Displays successful Windows logons using Event ID 4624.

**SPL:**

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4624
| stats count as successful_logins by Account_Name, Logon_Type
| sort - successful_logins
```

**Time range:** Last 24 hours

**Purpose:** Monitor successful authentication activity and identify which accounts and logon types are generating activity.

### 3. Failed Logins by Source IP

Groups failed Windows login attempts by source network address.

**SPL:**

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4625
| stats count as failed_logins by Source_Network_Address
| sort - failed_logins
```

**Time range:** Last 7 days

**Purpose:** Identify source IP addresses associated with failed authentication attempts.

### 4. Security Events Over Time

Displays the volume of Windows Security events over time.

**SPL:**

```spl
index=main sourcetype="WinEventLog:Security"
| timechart span=1h count
```

**Time range:** Last 24 hours

**Purpose:** Provide a timeline of security event activity and help identify unusual changes in event volume.

### 5. Security Events by Event ID

Shows the number of Windows Security events grouped by Event ID.

**SPL:**

```spl
index=main sourcetype="WinEventLog:Security"
| stats count by EventCode
| sort - count
```

**Time range:** Last 24 hours

**Purpose:** Provide an overview of which Windows Security event types are most common in the environment.

### 6. Credential Manager Activity

Displays Windows Credential Manager activity using Event ID 5379.

**SPL:**

```spl
index=main sourcetype="WinEventLog:Security" EventCode=5379
| rex "Account Name:\s+(?<account>\S+)"
| rex "Read Operation:\s+(?<operation>[^\r\n]+)"
| stats count by account, operation
| sort - count
```

**Time range:** Last 24 hours

**Purpose:** Monitor Credential Manager access and identify accounts performing credential-related operations.

### 7. Special Privileges Assigned

Displays Event ID 4672 activity grouped by account.

**SPL:**

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4672
| stats count by Account_Name
| sort - count
```

**Time range:** Last 24 hours

**Purpose:** Monitor logons where special privileges were assigned and provide additional context for privileged activity.

### 8. Account Changes

Displays Windows account-change activity using Event ID 4738.

**SPL:**

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4738
| stats count by Account_Name
| sort - count
```

**Time range:** Last 24 hours

**Purpose:** Monitor account-change activity and identify events that may require further investigation.

## Dashboard Design

The dashboard uses Splunk Dashboard Studio with a grid layout.

The panels provide different views of the collected Windows Security logs, including:

- Statistics tables
- Column charts
- Line charts
- Event counts
- Authentication activity
- Privileged activity
- Credential-related activity
- Account modifications

## Data Source

The dashboard uses Windows Security Event Logs collected from the Windows endpoint through the Splunk Universal Forwarder.

The logs are indexed in the Splunk `main` index with the following sourcetype:

```text
WinEventLog:Security
```

## Monitoring Workflow

The dashboard can be used as an initial SOC monitoring interface:

1. Review overall security event activity.
2. Check failed and successful authentication activity.
3. Review source IP addresses associated with failed logons.
4. Examine unusual event volumes or event types.
5. Review credential-related activity.
6. Review privileged logons.
7. Review account changes.
8. Investigate individual events using Splunk searches.

## Limitations

This dashboard was developed for a personal SOC home lab and focuses primarily on Windows Security Event Logs.

Automated alerting was not implemented because the lab uses the Splunk Free license.

The dashboard is intended for monitoring and investigation practice rather than production security operations.

## Status

Completed and tested
