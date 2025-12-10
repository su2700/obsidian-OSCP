
```
sc config winrm start= auto
sc start winrm
winrm quickconfig -q
netsh advfirewall firewall add rule name="WinRM 5985" dir=in action=allow protocol=TCP localport=5985

```

```
Set-Service -Name WinRM -StartupType Automatic
Start-Service WinRM
Enable-PSRemoting -Force
New-NetFirewallRule -DisplayName "WinRM 5985" -Direction Inbound -Protocol TCP -LocalPort 5985 -Action Allow

```

check 
```
netstat -ano | findstr 5985

```