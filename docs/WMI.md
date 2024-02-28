# WMI/WMIC

[Windows Management Instrumentation, Technique T1047 - Enterprise | MITRE ATT&CK®](https://attack.mitre.org/techniques/T1047/)

WMI is an administration feature that provides a uniform environment to access Windows system components. The WMI service enables both local and remote access, though the latter is facilitated by Remote Services such as Distributed Component Object Model (DCOM) and Windows Remote Management (WinRM). [1]  

## Source Host  

### 1. Windows Event Logs
   a. **4648**
      - A logon was attempted with explicit credentials  
         1) These events are generated when a process is attempting to logon by using explicit credentials. This is normally seen on scheduled tasks or by the use of the RUNAS command. This is also seen periodically during normal operating system activity.  
      - Currently Logged on username  
      - Alternate Username  
      - Destination Hostname/IP  
      - Process Name  

### 2. Process
   a. **Wmic.exe**
      - Wmic [Node:HOST process call create "c:\\temp\\evil.exe"]  
      - WMIC.exe is a native Windows utility. It has been started to be phased out on Windows 10 21H1 in favor of PowerShell. The WMI command-line (WMIC) utility provides a command-line interface for Windows Management Instrumentation (WMI).  
   b. **Powershell.exe**  
      - Invoke-WmiMethod -Computer HOST -Class win32_process -name create -Argument "c:\\temp\\evil.exe"  
      - Powershell.exe is a Windows Native executable. PowerShell is a cross-platform task automation solution made up of a command-line shell, a scripting language, and a configuration management framework, PowerShell runs on Windows, Linux, and macOS.  
   c. Commandline of just: "Wmic.exe"  
      - Will enable the interactive console.  
      - Will require extended script logging to be enabled beforehand to see the commands  

### 3. Network
   a. **Process: WMIC.exe**  
      - Outbound on destination port 445/135  
      - Source ports 59152-65535  


## Destination Host

### 1. Windows Event Logs
   a. **4624**  
      - An account successfully logged on  
      - Logon type 3: Network  
      - Source IP  
      - Logon Username  
      - This event generates when a logon session is created on a destination machine. It generates on the computer that was accessed, where the session was created. The logon type tells you what type of authentication it was made. On WMI and remote PowerShell being called remotely, it is Type 3 (Network).  
         1) Type 3 is seen on many different types of activities.  

   b. **4672**  
      - Special Privileges assigned to new logon  
         1) Logon username  
         2) Logon by user with administrative rights.  
            - If sensitive privileges are assigned to a new logon session, event 4672 is generated for that particular new logon. This event is very noisy for local system accounts.  

   c. Other minor events:
      - **5857, 5860, 5861**  

### 2. Process  
   a. **Evil.exe or cmd.exe**  
      - Parent Processes:  
         1) Srcons.exe  
         2) Mofcomp.exe  
         3) Wmiprvse.exe  
      - Sample commandline  
         - `Cmd.exe /c /q tasklist | findstr /I LSASS > \\\\127.0.0.1\\Admin$\\ 12345678910 2>&1`

### 3. Network
   a. **Process: "c:\\Windows\\system32\\svchost.exe -k netsvcs"**  
   b. **Process SYSTEM**  
      - Inbound on local port 445/135  
      - Remote ports 59152-65535  
