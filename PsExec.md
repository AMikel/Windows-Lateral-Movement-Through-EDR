# PsExec


[PsExec, Software S0029 | MITRE ATT&CK®](https://attack.mitre.org/software/S0029/)
[PsExec - Windows Sysinternals | Microsoft Docs](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)

Utilities like Telnet and remote control programs like Symantec's PC Anywhere let you execute programs on remote systems, but they can be a pain to set up and require that you install client software on the remote systems that you wish to access. PsExec is a light-weight telnet-replacement that lets you execute processes on other systems, complete with full interactivity for console applications, without having to manually install client software. PsExec's most powerful uses include launching interactive command-prompts on remote systems and remote-enabling tools like [IpConfig](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec) that otherwise do not have the ability to show information about remote systems.

## Source Host

### 1. Windows Event Logs  
   a. **4648**  
      - A logon was attempted with explicit credentials  
         1) These events are generated when a process is attempting to logon by using explicit credentials. This is normally seen on scheduled tasks or by the use of the RUNAS command. This is also seen periodically during normal operating system activity.  

### 2. Process  
   a. **PsExec.exe**  
      - Ran under the Source Username  
      - `psexec.exe "host" -accepteula -d -c c:\\temp\\EVIL.EXE`  
         1) The '-accepteula' flag is required the first time that this tool is ran.  
         2) This can be used to look for renamed PsExec instances.  
         3) False positives for the '-accepteula' can result for other Sysinternal tools like procmon.exe  
      - PsExec is not a native Windows tool and is not already present in the system. The file is part of the Sysinternals suite that has a lot of Windows utilities. These are now being maintained and distributed by Microsoft.  

### 3. Network
   a. **Process: psexec.exe**
      - Outbound on destination port 135 and 445  

### 4. File
   a. **PsExec.exe**
      - Is not a native Windows executable. Must be downloaded previous to execution  
      - File Metadata:  
	1)File hash  
	2) Description: "Remote Process Launcher"  
	3) Company: "Sysinternals"  
	4) Legal Copyright: "Mark Russinovich"  
	5) Internal Name: PsExec  

## Destination Host

### 1. Windows Event Logs
   a. **4648**
      - A logon was attempted with explicit credentials  
      - Connecting Username    
      - Process Name  
      - This event is logged on domain controllers only and both success and failure instances of this event are logged.  

   b. **4624**
      - An account successfully logged on  
      - Logon type 3: Network  
         1) Logon Type 2: Interactive if the flag '-u' is set for alternate credentials  
      - Logon Username  
      - This event generates when a logon session is created on a destination machine. It generates on the computer that was accessed, where the session was created. The logon type tells you what type of authentication it was made. On running PsExec, it is Type 3 (Network) normally.  
         1) Type 3 is seen on many different types of activities.  
         2) Type 2 logon is seen when a user is locally logged on the host. If the -u flag is set for alternate account with PsExec, this will be seen.  

   c. **4672**
      - Special Privileges assigned to new logon  
         1) Logon username  
         2) Logon by user with administrative rights.  
            a) Required for accessing the C$ and Admin$ share  
         3) If sensitive privileges are assigned to a new logon session, event 4672 is generated for that particular new logon. This event is very noisy for local system accounts.  

### 2. Services
   a. New service Creation  
      - `System\CurrentControlSet\services\PsExecSvc`  
         1) This can be renamed by using the '-r' flag  
      - 7045  
         a) Service Install  
      - 4679(S): A service was installed in the system?  
            1) Need to investigate if this event log record is created for the service  
            2) Service Name: PsExecSvc  
		i) This service name can be changed  


### 3. Process
   a. **PsExecsvc.exe, PSEXESVC.exe**  
      i. Not native to Windows. First time ran will be created at Admin$ along with any other executables ran along psexec.exe  
      ii. Ran under "NT Authority\System"  
      iii. Child processes are the command pushed out by PsExec and ran under the account that ran PsExec  

### 4. Network
   a. **SVCHost.exe + RPCSS**  
      i. Inbound on port 135  
      ii. SVCHost.exe is a native Windows process in charge of handling native windows service. The RPCSS flag is used for a lot of things.  

   b. **System**  
      i. Inbound on port 445  

