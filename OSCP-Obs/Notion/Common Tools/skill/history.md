how to get history use poweshell
```
Get-Content "$env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt"
```

旧版 PowerShell（无 PSReadLine）不会自动保存历史，因此没有历史文件。但你仍可通过：

Get-History


查看当前会话数据。

6. 在 Evil-WinRM 中访问历史记录

如果你的 shell 是 Evil-WinRM，你可以检查：
```
dir $env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\
```



然后：

type $env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt