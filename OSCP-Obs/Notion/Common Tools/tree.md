```
 Get-ChildItem -Path . -Depth 2 -Force
```


```
Get-ChildItem -Path C:\ -Depth 5 -Force -ErrorAction SilentlyContinue |
  Where-Object { $_.Name -match '(?i)\b(backup|old|password|pw|passwd|cpassword|secret)\b' } |
  Select-Object @{N='Type';E={if ($_.PSIsContainer) {'DIR'} else {'FILE'}}}, FullName, LastWriteTime, Length
```

```

Get-ChildItem -Path C:\Users\ -Depth 3 -Force -ErrorAction SilentlyContinue |
  Where-Object { $_.Name -match '(?i)\b(backup|old|password|pw|passwd|cpassword|secret)\b' } |
  Select-Object @{N='Type';E={if ($_.PSIsContainer) {'DIR'} else {'FILE'}}}, FullName, LastWriteTime, Length
```
