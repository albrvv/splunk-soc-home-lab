# Detection: Windows Account Changes

## Description

Detects Windows Event ID 4738, which is generated when changes are made to a user account.

This detection is used as a review trigger for unexpected account modifications and can help identify potentially suspicious changes to user accounts.

## Windows Event ID

**Event ID:** 4738  
**Event Name:** A user account was changed

## SPL Query

```spl
index=main sourcetype="WinEventLog:Security" EventCode=4738
| table _time, Message
| sort - _time
```

## Detection Logic

The search:

- Searches Windows Security logs.
- Filters for Event ID 4738.
- Displays the event timestamp and full event message.
- Sorts the newest events first.

The detection is intended to provide events for further investigation rather than automatically classify the account change as malicious.

## Lab Results

During testing, the search returned 10 Event ID 4738 events.

The events involved the local user account:

**Target Account:** albra

The subject associated with the events was:

**Subject Account:** ALBRAA$

## Investigation

The 10 events were reviewed to determine whether security-sensitive account attributes had changed.

The following fields were examined:

- Password Last Set
- Account Expires
- User Account Control
- AllowedToDelegateTo
- Other account attributes

No security-sensitive modifications were identified in the tested events.

The events therefore represent account-change activity that should be reviewed rather than evidence of malicious activity by itself.

## Why This Detection Matters

Unexpected account changes can be relevant to security monitoring because attackers may modify accounts to:

- Change account settings
- Modify privileges
- Maintain access
- Alter account expiration settings
- Change authentication-related properties

Event ID 4738 provides useful evidence for investigating these activities.

## Limitations

Event ID 4738 does not automatically indicate malicious activity.

Legitimate Windows administration and system processes can also generate account-change events. Additional context should be examined before determining whether an event is suspicious.

## Status

Tested and documented
