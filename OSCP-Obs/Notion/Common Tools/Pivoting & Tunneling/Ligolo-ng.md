

  

On Host1

```
root@vpn:/home/offsec# ./agent  -connect KaliIP:11601 -ignore-cert
```

  ```
   .\agentx64.exe -connect 192.168.45.180:11601 -ignore-cert
  ```

On Kali


```
ligolo-proxy -selfcert 
```


  

find internal IP for Kali: 172.16.213.122/24

so open new terminal on kali
since it is 122, need change to 0

```
 
 sudo ip route add  172.16.213.0/24 dev ligolo

```

  ```
   ip route list
  ```

back inside of ligolo


from kali you can accee any VM in internal


how to check tun
下面是对你这些命令的 **中文解释（清晰易懂）**：

---

## ✅ **1. `sudo modprobe tun`**

这条命令的作用是：

**加载内核的 TUN 模块。**

TUN 是一种虚拟网络接口，类似 VPN 需要用到的虚拟网卡。如果内核没有加载这个模块，你就无法创建 TUN 设备。

---

## ✅ **2. `ls -l /dev/net/tun || ( sudo mkdir -p /dev/net && sudo mknod /dev/net/tun c 10 200 && sudo chmod 666 /dev/net/tun )`**

这条命令的意思是：

### 先检查：

`ls -l /dev/net/tun`

如果这个文件存在 → 什么都不做。

### 如果这个文件不存在（命令返回错误），就执行括号里的：

#### （1）`sudo mkdir -p /dev/net`

创建目录 `/dev/net`（如果不存在）

#### （2）`sudo mknod /dev/net/tun c 10 200`

手动创建 TUN 设备文件：

- `c` = 字符设备
    
- `10 200` = TUN 设备的主从设备号
    

#### （3）`sudo chmod 666 /dev/net/tun`

将此设备设置为**所有用户可读写**，否则普通用户无法使用 TUN 设备。

👉 **这一步保证系统拥有可用的 `/dev/net/tun`，否则无法创建 tun 网卡。**


## ✅ **3. `sudo ip tuntap add dev ligolo mode tun`**

这条命令是：

**创建一个名为 `ligolo` 的 TUN 虚拟网络接口。**

- `ip tuntap` 用来管理 tun/tap 虚拟接口
    
- `dev ligolo` 创建名字叫 `ligolo` 的虚拟网卡
    
- `mode tun` 指明这是 TUN（不是 TAP）

```
sudo ip link set ligolo up
```

这条命令的意思是：

**把刚创建的 ligolo 虚拟网卡启动（启用）。**

如果不启动，它会保持 _DOWN_ 状态，不能使用。


## **`sudo ip route add 10.10.85.0/24 dev ligolo`**

这条命令是：

**添加一条路由：**

- 目标网段：`10.10.85.0/24`
    
- 通过接口：`ligolo`
    

作用是让你的电脑知道：

> “要访问 10.10.85.x 这个网段，请从 ligolo 虚拟网卡走。”

# **为什么 AutoRecon 会让 Ligolo 打出大量 “connection refused”？**

AutoRecon 在扫描某个目标时会做两件事情：

---

## **1）AutoRecon 会尝试不同端口，很多端口本来就没服务 → 拒绝 (TCP RST)**

AutoRecon 默认会扫：

- top 1000 TCP ports
    
- 一些 UDP ports
    
- 服务版本探测
    
- SMB、LDAP、Kerberos 等一大堆协议探测
    
- 各种 HTTP(s) 枚举请求
    

一个目标机器一般只有 **少量端口是开着的**。

其他关着的端口会直接返回：

`TCP RST → Connection refused`

Ligolo proxy 只是把这个事件打印出来。

---

## **2）AutoRecon 会发大量并发连接请求**

Ligolo 代理会看到：

- Nmap connect() 探测
    
- Banner grab
    
- NSE 脚本
    
- 服务探测
    

每当一个连接被远端拒绝，Ligolo Proxy 就会打印：

`connection was refused`


✅ 1. Delete the ligolo TUN interface (recommended)
sudo ip tuntap del dev ligolo mode tun


This is the correct way because:

It removes the TUN interface

It also removes all routes bound to ligolo

Works even when persist on

✅ 2. If the above fails, force delete via ip link

Try:

sudo ip link delete ligolo