# LSASS Credential Access Investigation

> **Dataset:** Metasploit LSASS  
> **Host:** `MKT01.pandalab.com`  
> **Telemetry:** Sysmon, Windows endpoint logs, and Zeek

## Summary

I investigated endpoint and network telemetry around `winx64_payload.exe` to determine whether the activity was related to credential access.

The investigation identified a suspicious payload followed by system discovery, internal network communication, malicious DLL staging and execution through `rundll32.exe`, and direct access to `lsass.exe`.

The evidence strongly supports **suspicious credential-access activity**, but I did not find enough evidence to confirm that credentials were successfully extracted.

## Attack Flow

```text
winx64_payload.exe
        |
        v
Process / service / system discovery
        |
        v
Internal network communication
        |
        v
rtcpef.dll created
        |
        v
rundll32.exe executes rtcpef.dll
        |
        v
winx64_payload.exe accesses lsass.exe
        |
        v
Credential-access investigation
```
## Key Findings
### 1. Suspicious Payload Execution
winx64_payload.exe executed from:

 ` C:\Users\pedro.gustavo\Downloads\`
 
 <img width="1842" height="399" alt="winxxxsuspicous" src="https://github.com/user-attachments/assets/b19f77c7-4af9-49b6-aba6-87d805873656" />
 
 <img width="1749" height="379" alt="winxx" src="https://github.com/user-attachments/assets/3031f6b7-7e3c-4f55-ad36-240a5d3ea6e7" />
 The same process was later associated with discovery activity, network communication, file creation, and process access.

### 2. Internal Network Activity
telemetry showed communication with the internal host:
`10.0.5.13:8080`
