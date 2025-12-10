ssh port forward

```
ssh -N -L 0.0.0.0:25985:10.10.185.142:5985 Eric.Wallows@192.168.225.141

```
- 在本地机器上打开一个 SSH 隧道，把本地监听的 25985 端口（绑定到所有网卡 `0.0.0.0`）的连接，透过 SSH 服务器 `192.168.225.141` 转发到目标内网主机 `10.10.185.142` 的 5985 端口（通常是 Windows 的 WinRM 服务端口）。
    

2. 各字段解释
    

- `ssh`：启动 OpenSSH 客户端。
    
- `-N`：不执行远端命令（只建立隧道，不打开远程 shell）。常用于端口转发。
    
- `-L 0.0.0.0:25985:10.10.185.142:5985`：本地端口转发选项，格式为 `-L [本地绑定地址:]本地端口:目标地址:目标端口`。
    
    - `0.0.0.0`：本地绑定地址，表示在本地的所有网络接口上监听（外部主机可以访问这个端口）。
        
    - `25985`：本地监听端口。
        
    - `10.10.185.142`：最终目标主机（从 SSH 服务器可访问的内网地址）。
        
    - `5985`：目标端口（WinRM 的默认端口）。
        
- `Eric.Wallows@192.168.225.141`：用 `Eric.Wallows` 这个用户去连接 SSH 服务器 `192.168.225.141`。
    

3. 关于 `0.0.0.0`（监听所有接口）与安全
    

- 默认情况下，SSH 本地转发通常只在 `127.0.0.1`（本机环回）上监听，只有本机可以访问。把绑定地址设成 `0.0.0.0` 会让隧道对同一网络中的其他机器可见/可连入，存在被未授权访问的风险。
    
- 要让 SSH 允许远程主机连接到本地转发端口，通常需要在本地客户端使用 `-g`（allow remote hosts to connect to local forwarded ports），或在 SSH 服务器的 `/etc/ssh/sshd_config` 中开启 `GatewayPorts yes`（改变后需重启 sshd）。不同 OpenSSH 版本对 `0.0.0.0:...` 的支持也略有差异。
    
- 更安全的做法：如果只是本机需要访问，改用 `127.0.0.1` 绑定；或者通过防火墙限制可以访问该端口的 IP。



## Pivoting through SSH

[](https://github.com/saisathvik1/OSCP-Cheatsheet#pivoting-through-ssh)

ssh adminuser@10.10.155.5 -i id_rsa -D 9050 #TOR port

#Change the info in /etc/proxychains4.conf also enable "Quiet Mode"

proxychains4 crackmapexec smb 10.10.10.0/24 #Example