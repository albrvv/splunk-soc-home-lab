# Windows Security Event Codes

## Overview

This document describes the main Windows Security Event IDs observed during the SOC home lab.

The events were collected from the Windows endpoint using the Splunk Universal Forwarder and indexed in Splunk.

## Authentication Events

### Event ID 4624 — Successful Logon

Indicates that a user successfully logged on to Windows.

**SOC relevance:**
- Monitor successful authentication activity.
- Identify unusual accounts or logon types.
- Correlate with privileged activity and other security events.

---

### Event ID 4625 — Failed Logon

Indicates that an attempted Windows logon failed.

**SOC relevance:**
- Detect repeated authentication failures.
- Investigate possible brute-force activity.
- Identify suspicious source addresses.
- Correlate multiple failures within a short time period.

This event is used by the **Multiple Failed Windows Logins** detection.

---

### Event ID 4634 — Logoff

Indicates that a logon session was terminated.

**SOC relevance:**
- Provides additional context for authentication investigations.
- Can be correlated with successful logon events.

---

### Event ID 4647 — User-Initiated Logoff

Indicates that a user initiated a logoff.

**SOC relevance:**
- Helps establish the timeline of user sessions.
- Can be correlated with Event ID 4624.

---

### Event ID 4648 — Explicit Credentials Used

Indicates that a process attempted to log on by explicitly specifying credentials.

**SOC relevance:**
- Can provide context for credential-use investigations.
- May be important when correlated with suspicious processes or accounts.

## Privilege and Account Events

### Event ID 4672 — Special Privileges Assigned to New Logon

Indicates that special privileges were assigned to a new logon.

**SOC relevance:**
- Monitor privileged logon activity.
- Investigate unexpected privileged access.
- Correlate with successful logon events and account activity.

This event is used by the **Special Privileges Assigned** detection.

---

### Event ID 4738 — A User Account Was Changed

Indicates that changes were made to a user account.

**SOC relevance:**
- Monitor account modifications.
- Investigate unexpected changes to account properties.
- Review changes involving authentication, privileges, or account settings.

This event is used by the **Windows Account Changes** detection.

## Account Enumeration Events

### Event ID 4798 — A User's Local Group Membership Was Enumerated

Indicates that a process or user queried the local group memberships of a user.

**SOC relevance:**
- Can provide visibility into account and privilege discovery.
- May be useful when investigating reconnaissance activity.

---

### Event ID 4799 — A Security-Enabled Local Group Membership Was Enumerated

Indicates that membership information for a security-enabled local group was queried.

**SOC relevance:**
- Can provide visibility into local privilege and group discovery.
- Should be investigated in context with other events.

---

### Event ID 4797 — An Attempt Was Made to Query Whether a User Account Exists Without a Password

Indicates an attempt to determine whether an account exists without supplying a password.

**SOC relevance:**
- Can provide additional context during account discovery investigations.

## Credential Manager Events

### Event ID 5379 — Credential Manager Credentials Were Read

Indicates that Windows Credential Manager credentials were accessed.

**SOC relevance:**
- Monitor credential-related activity.
- Investigate unexpected access to stored credentials.
- Correlate with the associated account, logon session, process, and other events.

During the lab investigation, high-volume Event ID 5379 activity was correlated with Windows Connected Devices Platform activity and was not determined to be malicious based on the available evidence.

---

### Event ID 5382 — Credential Manager Credentials Were Backed Up

Indicates activity involving the backup of Credential Manager credentials.

**SOC relevance:**
- Provides visibility into credential-related operations.
- Should be reviewed when associated with unexpected accounts or processes.

## Cryptographic Events

### Event ID 5058 — Key File Operation

Indicates an operation involving a cryptographic key file.

**SOC relevance:**
- Can provide context during investigations involving credential or certificate activity.
- Useful when correlated with process and account information.

---

### Event ID 5059 — Key Migration Operation

Indicates a cryptographic key migration operation.

**SOC relevance:**
- Provides additional context for cryptographic and credential-related activity.

---

### Event ID 5061 — Cryptographic Operation

Indicates a cryptographic operation involving a key.

**SOC relevance:**
- Can be correlated with credential, certificate, and process activity.

## Security Audit Events

### Event ID 4907 — Auditing Settings Changed

Indicates that auditing settings were changed.

**SOC relevance:**
- Changes to auditing configuration can affect security visibility.
- Unexpected changes should be investigated.

---

### Event ID 4616 — System Time Was Changed

Indicates that the system time was changed.

**SOC relevance:**
- Time changes can affect event timelines and log correlation.
- Unexpected changes should be investigated.

---

### Event ID 4904 — Security Event Source Registered

Indicates an attempt to register a security event source.

**SOC relevance:**
- Provides visibility into changes involving security event sources.
- Should be reviewed when unexpected.

---

### Event ID 4905 — Security Event Source Unregistered

Indicates an attempt to unregister a security event source.

**SOC relevance:**
- Provides visibility into changes involving security event sources.

## Events Observed in the Lab

The following Event IDs were observed in the collected Windows Security logs:

| Event ID | Description |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Logoff |
| 4647 | User-initiated logoff |
| 4648 | Explicit credentials used |
| 4672 | Special privileges assigned |
| 4738 | User account changed |
| 4797 | User account existence/password query |
| 4798 | User local group membership enumerated |
| 4799 | Security-enabled local group enumerated |
| 4904 | Security event source registered |
| 4905 | Security event source unregistered |
| 4907 | Auditing settings changed |
| 5058 | Cryptographic key file operation |
| 5059 | Cryptographic key migration |
| 5061 | Cryptographic operation |
| 5379 | Credential Manager credentials read |
| 5382 | Credential Manager credentials backed up |

## Notes

Event IDs should not be interpreted as malicious activity by themselves.

Security investigations should consider additional context such as:

- Account
- Source IP address
- Logon type
- Process
- Process ID
- Timestamp
- Related Event IDs
- Expected behavior in the environment

Events should be correlated before determining whether activity is suspicious.
