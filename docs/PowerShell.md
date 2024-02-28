# PowerShell Remoting


[Command and Scripting Interpreter: PowerShell, Sub-technique T1059.001 - Enterprise | MITRE ATT&CK®](https://attack.mitre.org/techniques/T1059/001/)

Adversaries may abuse PowerShell commands and scripts for execution. PowerShell is a powerful interactive command-line interface and scripting environment included in the Windows operating system. [1] Adversaries can use PowerShell to perform a number of actions, including discovery of information and execution of code. Examples include the Start-Process cmdlet which can be used to run an executable and the Invoke-Command cmdlet which runs a command locally or on a remote computer (though administrator permissions are required to use PowerShell to connect to remote systems).  

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
   a. **Powershell.exe**  
      - Powershell.exe -computername HOST  
      - Powershell.exe Invoke-command -computername HOST -scriptblock {start-process c:\temp\evil.exe}  
      - Commandline just: powershell.exe  
         1) Will enable the interactive console.  
         2) This will require extended script logging to be enabled beforehand to see the commands  
      - Powershell.exe is a Windows Native executable. PowerShell is a cross-platform task automation solution made up of a command-line shell, a scripting language, and a configuration management framework. PowerShell runs on Windows, Linux, and macOS.  

### 3. Network  
   a. **Process: Powershell.exe**  
      - Outbound on destination port 5985 (WinRM)  


## Destination Host  

### 1. Windows Event Logs  
   a. **4624**  
      - An account successfully logged on  
      - Logon type 3: Network  
      - Source IP  
      - Logon Username  
      - This event generates when a logon session is created on a destination machine. It generates on the computer that was accessed, where the session was created. The logon type tells you what type of authentication it was made. On remote PowerShell being called remotely, it is Type 3 (Network).  
         1) Type 3 is seen on many different types of activities.  

   b. **4103, 4104 – Script Block logging**
      - Logs suspicious scripts by default in PS v5 Logs all scripts if configured  
      - Will record what was ran on a PowerShell interactive shell that is not saved or seen on the commandline.  
      - Interactive sessions through the PowerShell ISE are bugged and will not be saved. No workaround and no plans to fix this.  

### 2. Process
   a. **Powershell.exe**  
      1) Parent Process:  
         - Wsmprovhost.exe  
      2) Child process  
         - Evil.exe  
         - Invoke-command  
            a) `Powershell.exe -Invoke-command -computername HOST -scriptblock {start-process c:\\temp\\evil.exe}`  
      3) Interactive sessions will require extended script logging to be enabled beforehand to see the commands on Event logs 4103-4104.  
      - Interactive sessions through the PowerShell ISE are bugged and will not be saved. No workaround and no plans to fix this.  

### 3. File
   a. **Evil.exe**  
      - PSsession will create a user profile directory may be created the first time it is ran.  

### 4. Network
   a. **Process: Powershell.exe**  
      - Inbound on local port 5985  
   b. **Process SYSTEM**  
      - Inbound on local port 445/135?  
![image](https://github.com/AMikel/Windows-Lateral-Movement-Through-EDR/assets/14891345/297e4fbb-1716-4f05-9b09-16bad66b9f08)  
