```
sudo lsof -i :8080
```

```
- netstat -ano | findstr :8000
```

```
sudo ss -tlnp '( sport = :80 )'
```
for windows
```
netstat -abon | findstr :135

```
you will get ps number like 884
then 

```
tasklist /svc /fi "pid eq 884"
```
or one cmd
```
for /f "tokens=5" %a in ('netstat -ano ^| findstr :135') do tasklist /svc /fi "pid eq %a"
```
in pwsh
```
Get-NetTCPConnection -LocalPort 135 | Select-Object LocalPort, OwningProcess, @{Name="ServiceNames"; Expression={(Get-WmiObject Win32_Service -Filter "ProcessId = $($_.OwningProcess)").Name}}

```
