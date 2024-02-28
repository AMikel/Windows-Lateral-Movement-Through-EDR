# Mapping of Network Share to C$ or Admin$


Remote Services: SMB/Windows Admin Shares, Sub-technique T1021.002 - Enterprise | MITRE ATT&CK®  

Adversaries may use Valid Accounts to interact with a remote network share using Server Message Block (SMB). The adversary may then perform actions as the logged-on user. SMB is a file, printer, and serial port sharing protocol for Windows machines on the same network or domain. Adversaries may use SMB to interact with file shares, allowing them to move laterally throughout a network. Linux and macOS implementations of SMB typically use Samba.  

Windows systems have hidden network shares that are accessible only to administrators and provide the ability for remote file copy and other administrative functions. Example network shares include C$, ADMIN$, and IPC$. Adversaries may use this technique in conjunction with administrator-level Valid Accounts to remotely access a networked system over SMB.[1] to interact with systems using remote procedure calls (RPCs),[2] transfer files, and run transferred binaries through network share execution techniques that rely on authenticated sessions over SMB/RPC are Scheduled Task/Job, Service Execution, and Windows Management Instrumentation. Adversaries can also use NTLM hashes to access administrator shares on systems with Pass the Hash and certain configuration and patch levels.

From `<https://attack.mitre.org/techniques/T1021/002/>`  

## Source Host

### 1. Windows Event Logs  
   a. **4648**  
      - A logon was attempted with explicit credentials  
         - These events are generated when a process is attempting to logon by using explicit credentials. This is normally seen on scheduled tasks or by the use of the RUNAS command. This is also seen periodically during normal operating system activity.  
   b. **31001**   
      - Failed logon to destination  
         - Username for failed logons    
         - Reason code for failed logons    
         - Will be generated on failed logons. Does not indicate successful lateral movements.  

### 2. Process  
   a. **Net.exe, Net1.exe**  
      - Ran under the Source Username  
      - Sample commandline command  
         - `net use z:\\HOST\C$ /user:DOMAIN\USERNAME`  
      - Net.exe/Net1.exe are a native component of Windows with various different functions related to networking. These can be used to manage network shares with the 'use' flag.  

## Destination Host  

### 1. Windows Event Logs  
   a. **4624**  
      - Logon Type 3: Network  
      - Source IP  
      - Logon Username  
      - This event generates when a logon session is created on a destination machine. It generates on the computer that was accessed, where the session was created. The logon type tells you what type of authentication it was made. On mapping a network share, it is Type 3 (Network).  
         1) Type 3 is seen on many different types of activities.  

   b. **4672**  
      - Logon Username  
      - Logon by user with admin rights  
         1) Required for C$ and Admin$ shares  
         2) If sensitive privileges are assigned to a new logon session, event 4672 is generated for that particular new logon. This event is very noisy for local system accounts.  

   c. **4776**  
      - The domain controller attempted to validate the credentials for an account  
      - NTLM Logon if authenticating to local system  
      - Source Username  
      - Logon Username  
      - When a domain controller successfully authenticates a user via NTLM (instead of Kerberos), the DC logs this event. This specifies which user account who logged on (Account Name) as well as the client computer's name from which the user initiated the logon in the Workstation field.  

   d. **4768**  
      - A Kerberos authenticating ticket was requested  
      - Available only on domain controllers  
      - Source Username  
      - Logon Username  
      - This event is logged on domain controllers only and both success and failure instances of this event are logged.  

   e. **4769**  
      - A Kerberos authenticating ticket was requested  
      - Only on Domain controllers  
      - Service ticket granted if authenticating to domain controllers  
      - This lets you monitor the granting of service tickets. Service tickets are obtained whenever a user or computer accesses a server on the network.  
      - [Windows Security Log Event ID 4769 - A Kerberos service ticket was requested](ultimatewindowssecurity.com)  

### 3. Network  
   a. **System**  
      - Process: System  
      - Outbound on destination port 445?  
         1) Haven't 100% verified this one  

### 5140, 5145  
   - SMB Share network events. Minor events.  

### 2. Process  
   a. NONE  

### 3. File  
   a. File metadata might be available for files executed on the target system. They will be copied over to the target system into destination share path.  
      i. Files are copied over to target system.  
      ii. The time the file was copied is the File_Modified_Time.  

### 4. Network  
   a. System  
      i. Inbound on port 445?  
         1) Haven't been able to verify this 100%  

