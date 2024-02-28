# Scheduled Tasks

[Scheduled Task/Job: Scheduled Task, Sub-technique T1053.005 - Enterprise | MITRE ATT&CK®](https://attack.mitre.org/techniques/T1053/005/)

Adversaries are able to use the Windows Task scheduler for initial persistence into the host. There are two native Schedule binaries for Windows. `Schtasks.exe` and `at.exe`. Adversaries are also able to use the task scheduling functionality for running commands on an external hosts. The `at` utility is deprecated and is kept for legacy purposes.

## Source Host

### Windows Event Logs
- **4648**
  - A logon was attempted with explicit credentials
    - These events are generated when a process is attempting to logon by using explicit credentials. This is normally seen on scheduled tasks or by the use of the RUNAS command. This is also seen periodically during normal operating system activity.
  - Currently logged on username
  - Alternate Username
  - Destination Hostname/IP
  - Process Name

### Process
- **At.exe**
  - Ran under the Source Username
  - Example commandline: `At \\HOST 13:00 "c:\\temp\\EVIL.exe"`
  - `At.exe` is an old deprecated utility to manage scheduled tasks in Legacy Windows. This is still natively present in modern Windows.
- **Schtasks.exe**
  - Ran by the source username
  - Example Commandline: `Schtasks.exe /create /TN TASKNAME /TR "c:\\temp\\EVIL.exe" /SC once /RU "System" /ST 13:00`
  - `Schtasks.exe` Native Windows executable for managing scheduled tasks. It has a lot of commandline options and flags to modify its behavior.

### Network
- **Process: at.exe**
  - Outbound on destination port 445?
    - Not 100% verified
- **Process: schtasks.exe**
  - Outbound on port 135

## Destination Host

### Windows Event Logs
- **4624**
  - An account successfully logged on
  - Logon type 3: Network
  - Source IP
  - Logon Username
  - This event generates when a logon session is created on a destination machine. It generates on the computer that was accessed, where the session was created. The logon type tells you what type of authentication it was made. On running Scheduled tasks remotely, it is Type 3 (Network).
    - Type 3 is seen on many different types of activities.
- **4672**
  - Special Privileges assigned to new logon
    - Logon username
    - Logon by user with administrative rights.
      - Required for accessing the C$ and Admin$ share
    - If sensitive privileges are assigned to a new logon session, event 4672 is generated for that particular new logon. This event is very noisy for local system accounts.
- Other minor event log types that could be used to track what scheduled task was created or modified:
  - **4698** – Scheduled task created
  - **4702** – Scheduled task updated
  - **4699** – Scheduled task deleted
  - **4700/4701** – Scheduled task enabled/disabled
  - **106** – Scheduled task created
  - **141** – Scheduled task updated
  - **140** – Scheduled task deleted
  - **200/201** – Scheduled task executed/completed

### Process
- **Evil.exe**
  - TODO
    - Parent Process?
    - Difference between at.exe and schtasks.exe and at.exe?

### File
- Job files created in `C:\Windows\Tasks`

### Network
- **Process: at.exe**
  - Inbound on port 445?
    - Have not verified 100% 
- **Process: schtasks.exe**
  - Inbound on port 135
 
  - 
