send windows. cmd
```
 net share pubshare="C:\Users\Administrator\desktop" /grant:everyone,full
```
pwsh
```
New-SmbShare -Name "ShareTest" -Path "C:\Users\Public\ShareTest" -FullAccess "Everyone"

```



receiver windows, login use user and pass
```
net use \\10.10.161.141\pubshare /user:MS01\Administrator December31

```
copy 
```
copy \\10.10.161.141\pubshare\winaudit.ps1 C:\Windows\Temp\

```

login and copy 
```
net use \\10.10.161.141\pubshare /user:MS01\Administrator December31; copy \\10.10.161.141\pubshare\winaudit.ps1 C:\Windows\Temp\

```

copy all 
```
xcopy \\10.10.161.141\ShareT
est C:\Users\celia.almeda\Documents  /E /I
```