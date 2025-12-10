```
shell Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server" -Name fDenyTSConnections -Value 0 -Force Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name UserAuthentication -Value 1 -Force netsh.exe advfirewall firewall add rule name="Open RDP Port 3389" dir=in action=allow protocol=TCP localport=3389
```

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server" -Name fDenyTSConnections -Value 0 -Force
```

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name UserAuthentication -Value 1 -Force
```

```
netsh.exe advfirewall firewall add rule name="Open RDP Port 3389" dir=in action=allow protocol=TCP localport=3389
```

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server" -Name fDenyTSConnections -Value 0 -Force  
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name UserAuthentication -Value 1 -Force  
netsh.exe advfirewall firewall add rule name="Open RDP Port 3389" dir=in action=allow protocol=TCP localport=3389  
  
xfreerdp /cert-ignore /u:sara /v:192.168.186.121
```

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server" -Name fDenyTSConnections -Value 0 -Force
```

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name UserAuthentication -Value 1 -Force
```

```
netsh.exe advfirewall firewall add rule name="Open RDP Port 3389" dir=in action=allow protocol=TCP localport=3389
