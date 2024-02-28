# Services  

[Create or Modify System Process: Windows Service, Sub-technique T1543.003 - Enterprise | MITRE ATT&CK®](https://attack.mitre.org/techniques/T1543/003/)

Adversaries are able to create or modify

Adversaries may create or modify Windows services to repeatedly execute malicious payloads as part of persistence. When Windows boots up, it starts programs or applications called services that perform background system functions.[1] Windows service configuration information, including the file path to the service's executable or recovery programs/commands, is stored in the Windows Registry. Service configurations can be modified using utilities such as sc.exe and Reg.

## Source Host

### Windows Event Logs
- NONE
- TODO: Further investigation if any Win events are generated as a result.

### Process
- **SC.exe**
  - Ran under the Source Username
  - Commands used:
    - `SC.exe \\HOST create SERVICENAME binpath= "c:\\temp\\evil.exe"`
    - `SC.exe \\HOST start SERVICENAME`
  - SC.exe is a command-line program used for communicating with the Service Control Manager and services. It is a native Windows executable that is used to control and manage Services in Windows.

### Network
- **Process: sc.exe**
  - Outbound on port 135

## Destination Host

### Windows Event Logs
- **4624**
  - An account successfully logged on
  - Logon type 3: Network
  - Source IP
  - Logon Username
  - This event generates when a logon session is created on a destination machine. It generates on the computer that was accessed, where the session was created. The logon type indicates the type of authentication that was made. On services being created remotely, it is Type 3 (Network).
    - Type 3 is seen on many different types of activities.

- Other minor Event Logs that could be used to further investigate what service was created or modified:
  - **4697** - Security records service install (Not enabled by default)
  - **7034** – Service crashed unexpectedly
  - **7035** – Service sent a Start/Stop control
  - **7036** – Service started or stopped
  - **7040** – Start type changed (Boot | On Request | Disabled)
  - **7045** – A service was installed on the system

### Process
- **Evil.exe**
  - Parent Process: Services.exe

### Network
- **Process: c:\Windows\system32\svchost.exe -k RPCSS**
  - Inbound on port 135
  - SVCHost.exe is a native Windows process in charge of handling native Windows service. The RPCSS flag is used for various purposes.
