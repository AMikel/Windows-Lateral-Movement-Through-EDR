# Services  

[Create or Modify System Process: Windows Service, Sub-technique T1543.003 - Enterprise | MITRE ATT&CK®](https://attack.mitre.org/techniques/T1543/003/)

Adversaries are able to create or modify

Adversaries may create or modify Windows services to repeatedly execute malicious payloads as part of persistence. When Windows boots up, it starts programs or applications called services that perform background system functions.[1] Windows service configuration information, including the file path to the service's executable or recovery programs/commands, is stored in the Windows Registry. Service configurations can be modified using utilities such as sc.exe and Reg.

## Source Host

### 1. Windows Event Logs
   a. NONE  
   b. TODO  
      - Further investigation if any Win events are generated as a result  

### 2. Process
   a. **SC.exe**  
      - Ran under the Source Username  
      - `SC.exe \\HOST create SERVICENAME binpath= "c:\\temp\\evil.exe"`  
      - `SC.exe \\HOST start SERVICENAME`  
      - SC.exe is not a native Windows executable that is used to control and manage Services in Windows.  

### 3. Network
   a. **Process: sc.exe**  
      - Outbound on port 135  


## Destination Host

### 1. Windows Event Logs
   a. **4624**
      - An account successfully logged on  
      - Logon type 3: Network  
      - Source IP  
      - Logon Username  
      - This event generates when a logon session is created on a destination machine. It generates on the computer that was accessed, where the session was created. The logon type tells you what type of authentication it was made. On services being created remotely, it is Type 3 (Network).  
         1) Type 3 is seen on many different types of activities.    

   b. Other minor Event Logs that could be used to further investigate what service was created or modified:  
      i. **4697** - Security records service install    
         1) Not enabled by default  
      ii. **7034** – Service crashed unexpectedly  
      iii. **7035** – Service sent a Start/Stop control  
      iv. **7036** – Service started or stopped  
      v. **7040** – Start type changed (Boot | On Request | Disabled)  
      vi. **7045** – A service was installed on the system  

### 2. Process
   a. **Evil.exe**  
      - Parent Process Services.exe  

### 3. Network
   a. **Process: c:\Windows\system32\svchost.exe -k RPCSS**  
      - Inbound on port 135  
      - SVCHost.exe is a native Windows process in charge of handling native windows service. The RPCSS flag is used for a lot of things.  
