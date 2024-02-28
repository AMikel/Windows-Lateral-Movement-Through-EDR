# Remote Services: Remote Desktop Protocol, Sub-technique T1021.001 - Enterprise | MITRE ATT&CK®

Adversaries may use Valid Accounts to log into a computer using the Remote Desktop Protocol (RDP). The adversary may then perform actions as the logged-on user. Remote desktop is a common feature in operating systems. It allows a user to log into an interactive session with a system desktop graphical user interface on a remote system. Microsoft refers to its implementation of the Remote Desktop Protocol (RDP) as Remote Desktop Services (RDS).[1]

Adversaries may connect to a remote system over RDP/RDS to expand access if the service is enabled and allows access to accounts with known credentials. Adversaries will likely use Credential Access techniques to acquire credentials to use with RDP. Adversaries may also use RDP in conjunction with the Accessibility Features technique for Persistence.

From `<https://attack.mitre.org/techniques/T1021/001/>`

## Source Host

### 1. Windows Event Logs
   a. **4648**
      - A logon was attempted with explicit credentials
         1) These events are generated when a process is attempting to logon by using explicit credentials. This is normally seen on scheduled tasks or by the use of the RUNAS command. This is also seen periodically during normal operating system activity.
   b. **1024, 1102**
      - These two are remote desktop events that can sometimes be seen when initiating a remote desktop session.

### 2. Process
   a. **MSTSC.exe**
      - Can run under the Source Username
      - Command line flags might contain
         1) Target Hostname
         2) Target IP
         3) Scp or local file, '.RDP' file type
      - MSTSC.exe is a native windows process. It creates connections to Remote Desktop Session Host servers or other remote computers.

### 3. Network
   a. **MSTSC.exe**
      - Outbound on destination port 3389
      - Outbound port can be changed

## Destination Host

### 1. Windows Event Logs
   a. **4624**
      - An account successfully logged on
      - Logon type 10: Remote Interactive
      - IP Address is the source
      - Logon Username
      - This event generates when a logon session is created on a destination machine. It generates on the computer that was accessed, where the session was created. The logon type tells you what type of authentication it was made on. On Remote desktop connections, it is type 10 (Remote Interactive).
         1) Type 10 is only seen on Remote Desktop, Terminal Services or Remote Assistance logons.
   b. **1149**
      - Source IP
      - Logon Username
      - Blank username might indicate Sticky Keys Exploit
      - This event is from the listener for Remote desktop on the target host. The event is generated when a remote desktop connection is generated.
   c. **4778, 4779, 131, 98**
      - These are other minor events that are generated from Remote Desktop activities.

### 2. Process
   a. **RDPclip.exe, tsTheme.exe**
      - Ran under the target username
      - Not part of the normal process tree, use a timeline search around the time the Remote desktop connected
      - RDPclip.exe is the process in charge of copy and pasting from the clipboard during RDP sessions. This is a native windows process. It only runs while RDP is in use.
      - TsTheme.exe is a native windows process. It is in charge of the Windows theme during a RDP session. It only runs while RDP is in use.

### 3. Network
   a. **SVCHost.exe -k termsvc**
      - Inbound on port 3389
      - The port can be changed
      - SVCHost.exe is a native Windows process in charge of handling native windows service. The termsvc flag indicates that this is in charge of RDP Connections. 
