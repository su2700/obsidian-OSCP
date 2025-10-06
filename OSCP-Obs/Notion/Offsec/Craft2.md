thecybergeek::CRAFT2:5371700931b77050:77E00463712E5DAF3F2EAA08EE62AADB:0101000000000000000FD66BD1FFDB01EA9A4BA1CD5755CE0000000002000800470036004500460001001E00570049004E002D00360046005A0051004B0059004800310046003900410004003400570049004E002D00360046005A0051004B005900480031004600390041002E0047003600450046002E004C004F00430041004C000300140047003600450046002E004C004F00430041004C000500140047003600450046002E004C004F00430041004C0007000800000FD66BD1FFDB010600040002000000080030003000000000000000000000000030000000EC7CA1882CB50297357209482D722C2F3D41A566D0CC3BD5A56B55A058CBB00A001000000000000000000000000000000000000900260063006900660073002F003100390032002E003100360038002E00340035002E003100380031000000000000000000

  
[[evil-winrm]]


To use **Evil-WinRM** (a tool for connecting to Windows Remote Management services), the following **ports must be open** on the target Windows machine:

---

### **Required Ports**

1. **WinRM (HTTP)** → **Port 5985** (Default for unencrypted HTTP traffic).
2. **WinRM (HTTPS)** → **Port 5986** (Default for encrypted HTTPS traffic).

- If the target uses **HTTPS (5986)**, add `**S**`:
    
    ```Plain
    bashCopy
    evil-winrm -i <TARGET_IP> -u <USERNAME> -p <PASSWORD> -S
    ```
    

```Shell
smbclient //$IP/WebApp -U thecybergeek%winniethepooh
```

```Plain
cmd /c sc qc Mysql

```

breaks down as follows:

- `**cmd /c**`: This tells Windows to open a new command prompt, run the command that follows, and then terminate the command prompt.
- `**sc**`: This is the Service Control command-line utility in Windows, used to communicate with the Service Control Manager and services.
- `**qc**`: This is a subcommand of `**sc**` that stands for **Query Configuration**. It retrieves and displays the configuration information of a specified Windows service.
- `**Mysql**`: This is the name of the service you want to query. In your case, it’s the MySQL service installed by XAMPP.