how to porward port


```
netsh interface portproxy add v4tov4 listenport=6666 listenaddress=0.0.0.0 connectport=5985 connectaddress=10.10.191.142

```
**本机**上创建一个端口转发（IPv4→IPv4）：把本机任意接口上监听的 **TCP 6666** 的流量转发到 **10.10.191.142:5985**（远端主机的 5985 端口，通常是 WinRM HTTP）。

- `netsh interface portproxy add v4tov4`：添加一个 IPv4 到 IPv4 的端口代理规则。
    
- `listenport=6666`：本机要监听的端口（外部或本机连接到此端口都会被转发）。
    
- `listenaddress=0.0.0.0`：监听所有本地 IPv4 地址（等同于任意网卡）；也可以指定具体 IP（例如 192.168.1.10）。
    
- `connectaddress=10.10.191.142`：代理目标地址（流量被转发到此 IP）。
    
- `connectport=5985`：代理目标端口（流量被转发到 5985）。
`netsh`（Network Shell）是 Windows 内置的命令行网络配置工具。它提供多个“上下文”（contexts），可以查看/修改网络接口、路由、防火墙、端口代理等各种网络相关设置。运行 `netsh` 需要管理员权限（以管理员身份打开命令提示符或 PowerShell）。

# `interface` 是什么意思

在 `netsh` 的命令结构里，`interface` 表示进入“网络接口”相关的配置上下文。很多与接口、IP、路由、端口代理相关的子命令都在这个上下文下。

# `portproxy` 是什么

`portproxy` 是 `netsh interface` 下的一个子功能，用来在本地机器上建立 TCP 端口转发（端口代理）。它可以把本机某个监听端口的传入 TCP 连接，转发到另一个 IP:端口上。注意 `portproxy` 只支持 **TCP**（不支持 UDP）。
how to delete
```
netsh interface portproxy delete v4tov4 listenaddress=0.0.0.0 listenport=6666

```
show all
```
netsh interface portproxy show all

```

how to open firewall

```
netsh advfirewall firewall add rule name="port_forward_6666" protocol=TCP dir=in localip=192.168.231.141 localport=6666 action=allow

```
how to delete
```
netsh advfirewall firewall delete rule name="port_forward_6666"
# 或 PowerShell
Remove-NetFirewallRule -DisplayName "port_forward_6666"

```


- `netsh advfirewall firewall add rule`：向 Windows Defender 防火墙添加一条规则。
    
- `name="port_forward_6666"`：规则名称（可自定义）。
    
- `protocol=TCP`：协议为 TCP。
    
- `dir=in`：入站方向（从外部到本机）。
    
- `localip=192.168.231.141`：只对本机的这个 IP 生效（多网卡时可指定某一地址）；省略则对所有本地 IP 生效。
    
- `localport=6666`：本地端口号 6666。
    
- `action=allow`：允许匹配流量通过（放行）。
    

**注意**：执行需管理员权限（以提升的 CMD / PowerShell 运行）。
`advfirewall` 指的是 **Windows 的進階防火牆（Windows Defender Firewall with Advanced Security）**。在 `netsh` 這個命令列工具裡，`advfirewall` 是一個子上下文，用來新增、查詢或移除防火牆規則與設定
---

## 生效要点

- 该规则仅放行入站 TCP 6666 到 `192.168.231.141`。若你已经用 `netsh interface portproxy` 做了端口转发（listen 6666 → 远端），这条防火墙规则允许外部主机连接到 6666。两者配合才能让外部访问成功。
    
- 如果你想允许任意本机 IP 上的 6666，请去掉 `localip=...` 或改为 `localip=0.0.0.0`。
    
- 如果仅想本机访问（不允许远程），把规则改为 `localip=127.0.0.1` 或省略并限制 `remoteip=127.0.0.1`。
