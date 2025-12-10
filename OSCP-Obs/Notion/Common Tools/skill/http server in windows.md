how to create http server use pwsh

- Short PowerShell HttpListener server (serves files from current directory): at port 8000

```

$listener = [System.Net.HttpListener]::new(); $listener.Prefixes.Add('http://+:8000/'); $listener.Start(); while ($true) { $ctx = $listener.GetContext(); $req = $ctx.Request; $path = $req.Url.AbsolutePath.TrimStart('/'); if ($path -eq '') { $path = '.' }; $fp = Join-Path (Get-Location) $path; if (-not (Test-Path $fp)) { $ctx.Response.StatusCode = 404; $ctx.Response.Close(); continue } $b = [System.IO.File]::ReadAllBytes($fp); $ctx.Response.ContentLength64 = $b.Length; $ctx.Response.OutputStream.Write($b,0,$b.Length); $ctx.Response.Close() }
```

simpleHTTPServer
```
# 后台启动的健壮 HttpListener（端口8000）——直接粘贴运行
$script = {
  $root = (Get-Location).Path
  $listener = New-Object System.Net.HttpListener
  $prefix = 'http://+:8000/'
  if (-not $listener.Prefixes.Contains($prefix)) { $listener.Prefixes.Add($prefix) }
  try {
    $listener.Start()
  } catch {
    Write-Error "Listener start failed: $_"
    return
  }
  Write-Output "SimpleHttpServer started on port 8000, root: $root"
  while ($listener.IsListening) {
    try {
      $ctx = $listener.GetContext()
    } catch { break }   # listener 已停止或异常，跳出循环
    try {
      $req = $ctx.Request
      $path = $req.Url.AbsolutePath.TrimStart('/')
      if ($path -eq '') { $path = 'index.html' }    # 默认 index.html（若无则返回 listing）
      $fp = Join-Path $root $path
      if (-not (Test-Path $fp)) {
        # 文件不存在：尝试列目录或返回 404
        if ($path -eq 'index.html') {
          # 列目录内容
          $files = Get-ChildItem -Path $root | Select-Object -ExpandProperty Name
          $html = "<html><body><h3>Index of $root</h3><ul>" + ($files | ForEach-Object { "<li><a href='$_'>$_</a></li>" }) -join '' + "</ul></body></html>"
          $buffer = [System.Text.Encoding]::UTF8.GetBytes($html)
          $ctx.Response.ContentType = "text/html; charset=utf-8"
          $ctx.Response.ContentLength64 = $buffer.Length
          $ctx.Response.OutputStream.Write($buffer,0,$buffer.Length)
          $ctx.Response.Close()
          continue
        } else {
          $ctx.Response.StatusCode = 404
          $ctx.Response.StatusDescription = "Not Found"
          $ctx.Response.Close()
          continue
        }
      }
      # 是文件：读取并返回（按字节）
      $bytes = [System.IO.File]::ReadAllBytes($fp)
      # 基于扩展名设置简单 mime
      $ext = [System.IO.Path]::GetExtension($fp).ToLower()
      switch ($ext) {
        '.txt' { $ctx.Response.ContentType = 'text/plain; charset=utf-8' }
        '.html' { $ctx.Response.ContentType = 'text/html; charset=utf-8' }
        default { $ctx.Response.ContentType = 'application/octet-stream' }
      }
      $ctx.Response.ContentLength64 = $bytes.Length
      $ctx.Response.OutputStream.Write($bytes,0,$bytes.Length)
      $ctx.Response.Close()
    } catch {
      # 捕获单次请求异常，继续服务
      Write-Error "Request error: $_"
      try { $ctx.Response.StatusCode = 500; $ctx.Response.Close() } catch {}
    }
  }
  $listener.Stop()
  $listener.Close()
}
Start-Job -ScriptBlock $script -Name SimpleHttpServer8000
Write-Output "Job started: SimpleHttpServer8000"

```
