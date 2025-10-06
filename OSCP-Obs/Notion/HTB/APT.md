
Starting Nmap 7.93 ( [https://nmap.org](https://nmap.org/) ) at 2023-08-20 09:11 EDT  
Nmap scan report for 10.10.10.213  
Host is up (0.034s latency).

PORT STATE SERVICE VERSION  
80/tcp open http Microsoft IIS httpd 10.0  
|_http-title: Gigantic Hosting | Home  
|_http-server-header: Microsoft-IIS/10.0_  
_| http-methods:_  
_|_ Potentially risky methods: TRACE  
135/tcp open msrpc Microsoft Windows RPC  [[135]]
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port  
Device type: general purpose  
Running (JUST GUESSING): Microsoft Windows 2016|2012|2008|10 (91%)  
OS CPE: cpe:/o:microsoft:windows_server_2016 cpe:/o:microsoft:windows_server_2012 cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_10:1607  
Aggressive OS guesses: Microsoft Windows Server 2016 (91%), Microsoft Windows Server 2012 (85%), Microsoft Windows Server 2012 or Windows Server 2012 R2 (85%), Microsoft Windows Server 2012 R2 (85%), Microsoft Windows Server 2008 R2 (85%), Microsoft Windows 10 1607 (85%)  
No exact OS matches for host (test conditions non-ideal).  
Network Distance: 2 hops  
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

TRACEROUTE (using port 80/tcp)  
HOP RTT ADDRESS  
1 34.85 ms 10.10.14.1  
2 34.95 ms 10.10.10.213

OS and Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .  
Nmap done: 1 IP address (1 host up) scanned in 16.35 seconds

[[smbclient]]


```JavaScript
smbclient.py is better than smbclient

tee // 
```

135 RPC, Remote Procedure Call[[135[[RPC]]]]

rpcclient[[rpcclient]]

rpcdump[[rpcdump.py]]

rpcmap

  

python3 [IOXIDResolver.py](http://ioxidresolver.py/) -t 10.10.10.213  
[*] Retrieving network interface of 10.10.10.213  
Address: apt  
Address: 10.10.10.213  
Address: dead:beef::59d0:ef2:7eef:1a40  
Address: dead:beef::b885:d62a:d679:573f

  

  

### Explanation of NTDS.dit File

1. **Contents**:
    - **User Accounts**: Includes details like usernames, group memberships, and user attributes.
    - **Password Hashes**: Stores hashed versions of user passwords.
    - **Group Memberships**: Information about which users belong to which groups.
    - **Group Policies**: Contains policies applied to users and computers in the domain.
    - **Other Attributes**: Such as logon scripts, home directories, and more.
2. **Location**:
    
    - The NTDS.dit file is typically located in the `%SystemRoot%\NTDS` directory on a Windows domain controller.
    
      
    
      [[secretsdump.py]]
    
    `secretsdump` is a tool from the Impacket toolkit, designed for extracting sensitive information from a variety of Windows authentication data sources, including the NTDS.dit file. Here's why it's a preferred tool: