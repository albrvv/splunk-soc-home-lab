# Investigation: Credential Manager Access

## Objective

Investigate Windows Credential Manager access events and determine whether the activity was expected or suspicious.

## Initial Detection

Windows Security Event ID **5379** was observed at a high frequency.

**Event 5379:** Credential Manager credentials were read.

Initial Splunk search:

```spl
index=main sourcetype="WinEventLog:Security" EventCode=5379
| rex "Account Name:\s+(?<account>\S+)"
| rex "Read Operation:\s+(?<operation>[^\r\n]+)"
| stats count by account, operation
| sort - count
```

## Observed Activity

The search produced:

| Account | Operation | Count |
|---|---|---|
| albra | Enumerate Credentials | 3,209 |
| ALBRAA$ | Enumerate Credentials | 50 |
| albra | Read Credential | 49 |
| LOCAL | Enumerate Credentials | 10 |

The high volume of Event 5379 activity required further investigation.

## Investigation

### 1. Correlate the Logon ID

A sample Event 5379 was identified with:

- **Account Name:** albra
- **Read Operation:** Enumerate Credentials
- **Logon ID:** 0x58F0574

The same Logon ID was searched across Security events.

This identified a related Event 4624:

- **Logon Type:** 11
- **New Logon Account:** albraa.haitham117@hotmail.com
- **Account Domain:** MicrosoftAccount
- **Logon ID:** 0x58F0574
- **Process Name:** C:\Windows\System32\svchost.exe
- **Source Address:** 127.0.0.1

### 2. Correlate Cryptographic Events

Additional events around the same activity were identified.

**Event 5058**

- **Process ID:** 18336
- **Key Name:** Microsoft Connected Devices Platform device certificate
- **Operation:** Read persisted key from file
- Key path was under the user's Microsoft Crypto keys directory.

**Event 5061**

- **Process:** Microsoft Software Key Storage Provider
- **Algorithm:** ECDSA_P256
- **Key Name:** Microsoft Connected Devices Platform device certificate
- **Operation:** Open Key

**Event 5059**

- **Process ID:** 18336
- Same Connected Devices Platform certificate
- **Operation:** Export of persistent cryptographic key

### 3. Identify the Process

A process lookup was performed for PID 18336.

PowerShell identified the process as:

```text
svchost
PID 18336
```

The Windows service associated with this process was then identified using:

```powershell
Get-CimInstance Win32_Service |
Where-Object {$_.ProcessId -eq 18336} |
Select-Object Name, DisplayName, State, StartMode
```

The result was:

```text
Name         : CDPUserSvc_58febd8
DisplayName  : Connected Devices Platform User Service_58febd8
State        : Running
StartMode    : Auto
```

## Investigation Conclusion

The high volume of Event 5379 activity was investigated and correlated with related authentication and cryptographic events.

The activity was associated with:

```text
svchost.exe
    |
    └── CDPUserSvc_58febd8
        Connected Devices Platform User Service
```

The related events involved a Microsoft Connected Devices Platform certificate.

Based on the evidence collected in this lab, the activity appeared consistent with legitimate Windows activity. No evidence from this investigation alone was sufficient to conclude that the system was compromised.

## Important Note

Event 5379 by itself is not proof of malicious credential access.

A SOC analyst should correlate:

- Account
- Logon ID
- Process
- Process ID
- Related authentication events
- Cryptographic events
- Timing
- Expected Windows services

before determining whether the activity is suspicious.

## Status

Investigated and documented.

The investigation demonstrated correlation of multiple Windows Security Event IDs and identification of the Windows service associated with the observed Credential Manager activity.
