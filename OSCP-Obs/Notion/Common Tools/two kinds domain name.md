
NetBOIS-Domain name AND Active Directory Domain DIFFERENT? 


## . **

```
spookysec.local
```

** - DNS Domain Name (FQDN)

- This is the **fully qualified domain name**
- Used for DNS resolution and Kerberos authentication
- Format: 
    
    ```
    domain.local
    ```
    
     or 
    
    ```
    domain.com
    ```
    
- Shown by nxc/LDAP

## 2. **

```
THM-AD
```

** - NetBIOS Domain Name

- This is the **short/legacy domain name**
- Used for older Windows protocols (SMB, NetBIOS)
- Format: Usually uppercase, max 15 characters
- Shown by rpcclient/lookupsid

## Why both exist?

**Always use dns first, then may try netbios 


Windows domains have **both names** for compatibility:

- **DNS name** (
    
    ```
    spookysec.local
    ```
    
    ) - Modern, used by Active Directory
- **NetBIOS name** (
    
    ```
    THM-AD
    ```
    
    ) - Legacy, used by older Windows systems

是的，它们**完全不同**，但通常有关联。

在 Active Directory (AD) 中，每一个域实际上都有**两个名字**。这通常是历史原因造成的（为了兼容旧版 Windows）。

简单来说：

- **AD 域名 (DNS Domain Name)**：是域的**正式全名**（像身份证上的名字）。
    
- **NetBIOS 域名**：是域的**旧版简称/绰号**（像朋友叫你的昵称）。
    

以下是详细对比：

### 1. 核心区别对比

|特性|**Active Directory 域名 (DNS Name)**|**NetBIOS 域名**|
|---|---|---|
|**例子**|`vulnnet-rst.local`|`VULNNET-RST`|
|**结构**|**分层结构**（用点 `.` 分隔），像网站域名。|**扁平结构**（没有点），单纯的字符串。|
|**长度限制**|很长。|**非常短**（最多 15 个字符）。|
|**用途**|现代服务：LDAP, Kerberos, DNS 解析。|旧版兼容：SMB 共享, NTLM 认证。|
|**登录格式**|`user@vulnnet-rst.local` (UPN)|`VULNNET-RST\user` (Down-Level)|

---

### 2. 为什么会有两个名字？

**历史包袱。** 在 Windows 2000 之前，网络不支持像 DNS 那样的长域名，只能用短的大写字母名字（即 NetBIOS）。 现代 Windows 为了兼容旧软件和设备，保留了这个“短名”。

- **默认情况**：当你创建一个新域 `vulnnet-rst.local` 时，Windows 会自动截取第一个点前面的部分作为 NetBIOS 名，即 `VULNNET-RST`。
    
- **特殊情况**：管理员完全可以把它们设置成**不一样**的名字！
    
    - AD 域名: `internal.corp.bank.com`
        
    - NetBIOS 域名: `BANK` (为了方便输入)
        

### 3. 对渗透测试/黑客的影响 (为什么你要在乎？)

在攻击或枚举时，搞混这两个名字会导致工具报错。

#### A. 登录格式 (最重要的区别)

当你使用 `smbclient`, `crackmapexec`, 或 `xfreerdp` 时，不同的写法对应不同的协议处理：

- **格式 1 (NetBIOS 风格):** `DOMAIN\User`
    
    - 例如：`VULNNET-RST\Administrator`
        
    - 这是传统的 Windows 登录方式 (Down-Level Logon Name)。
        
- **格式 2 (UPN 风格):** `User@DNS_Domain_Name`
    
    - 例如：`Administrator@vulnnet-rst.local`
        
    - 这是现代的登录方式 (User Principal Name)。
        

#### B. 工具参数

有些工具（如 Impacket）比较智能，能自动识别；但有些工具必须精准区分：

- **Kerberos 攻击**（如 Golden Ticket）通常需要**FQDN** (即完整的 AD 域名 `vulnnet-rst.local`)。
    
- **SMB 枚举**有时只需要短的 NetBIOS 名。
    

### 总结

在您的案例中：

- **AD 域名**: `vulnnet-rst.local` (用于 LDAP 查询，DNS)
    
- **NetBIOS 域名**: 很可能是 `VULNNET-RST` (用于 `DOMAIN\User` 登录)
    

**建议：** 如果您在使用工具爆破或登录时失败，尝试切换这两种用户名的写法，通常能解决问题。

how to get them? which tool is for which

获取这两个名字并不难，通常我们可以通过**SMB**（端口 445）或 **LDAP**（端口 389）协议来查询。

以下是针对不同目标的最佳工具和命令，我为您整理了一个清单：

### 1. 获取 NetBIOS 域名 (短名)

NetBIOS 域名主要通过 SMB 协议获取。

#### **工具 A: smbclient (最直接)**

这是您之前已经在用的工具。当您列出共享时，服务器通常会在第一行自报家门。

- **命令:**
    
    Bash
    
    ```
    smbclient -L //10.64.148.111 -N
    ```
    
    _( `-L` 列出共享, `-N` 匿名登录)_
    
- **看哪里:** 输出结果的第一行通常长这样： `Domain=[VULNNET-RST] OS=[Windows Server 2019...]`
    
    - 这里的 `[VULNNET-RST]` 就是 **NetBIOS 域名**。
        

#### **工具 B: nmblookup (专门查 NetBIOS)**

这是 Linux 上专门用来查 NetBIOS 信息的原生工具。

- **命令:**
    
    Bash
    
    ```
    nmblookup -A 10.64.148.111
    ```
    
- **看哪里:** 输出中标记为 `<00>` 且类型为 `GROUP` 的行通常就是域名。
    

---

### 2. 获取 Active Directory 域名 (长名/DNS名)

这个名字主要通过 LDAP 协议获取，或者通过 DNS 反查。

#### **工具 A: ldapsearch (最标准)**

您最开始使用的命令就已经拿到了这个信息。

- **命令:**
    
    Bash
    
    ```
    ldapsearch -x -H ldap://10.64.148.111 -s base namingContexts
    ```
    
- **看哪里:** 看 `DC=vulnnet-rst,DC=local`。 将 `DC` 拼起来就是 `vulnnet-rst.local`。
    

#### **工具 B: nslookup (如果有 DNS 权限)**

如果您知道 DNS 服务器的 IP，可以直接问它“你是谁”。

- **命令:**
    
    Bash
    
    ```
    nslookup 10.64.148.111 10.64.148.111
    ```
    

---

### 3. 一次性获取两者 (Best for Lazy Hackers)

最推荐使用自动化工具，它们会帮您把两个名字都列出来，方便对比。

#### **工具 A: CrackMapExec / NetExec (nxc) (渗透神器)**

这是内网渗透中最常用的工具，它会自动探测并显示域信息。

- **命令:**
    
    Bash
    
    ```
    # 使用 smb 协议探测
    nxc smb 10.64.148.111
    # 或者旧版
    crackmapexec smb 10.64.148.111
    ```
    
- **看哪里:** 输出通常是黄色的： `SMB 10.64.148.111 445 DC01 [*] Windows Server ... (name:DC01) (domain:vulnnet-rst.local)`
    
    - 注意：CME/NXC 通常显示的是 **DNS 域名** (`vulnnet-rst.local`)。
        
    - 如果它显示 `signing:True`，说明这确实是域控制器。
        

#### **工具 B: Nmap (通过脚本)**

Nmap 的脚本引擎非常强大，可以把两个名字写的清清楚楚。

- **命令:**
    
    Bash
    
    ```
    nmap -p 445 --script smb-os-discovery 10.64.148.111
    ```
    
- **看哪里:** 输出会非常详细：
    
    Plaintext
    
    ```
    | smb-os-discovery:
    |   OS: Windows Server 2019 ...
    |   Computer name: DC01
    |   NetBIOS computer name: DC01\x00
    |   Domain name: vulnnet-rst.local    <-- AD 域名 (DNS)
    |   Forest name: vulnnet-rst.local
    |   FQDN: DC01.vulnnet-rst.local
    |   NetBIOS domain name: VULNNET-RST  <-- NetBIOS 域名
    ```
    

### 总结对照表

|您想知道什么？|**Active Directory 域名 (DNS)**|**NetBIOS 域名**|
|---|---|---|
|**典型样子**|`vulnnet-rst.local`|`VULNNET-RST`|
|**首选工具**|`ldapsearch`, `nmap`|`smbclient`, `nmblookup`|
|**用途**|LDAP 查询, Kerberos 票据|`DOMAIN\User` 登录格式|

**建议下一步：** 既然您手里有 `nxc` (NetExec)，直接运行 `nxc smb 10.64.148.111` 是最快确认目标环境的方法。

  

➜  nxc-auto.sh git:(main) ✗ nxc smb  10.64.182.102

SMB         10.64.182.102   445    ATTACKTIVEDIREC  [*] Windows 10 / Server 2019 Build 17763 x64 (name:ATTACKTIVEDIREC) (domain:spookysec.local) (signing:True) (SMBv1:False)

➜  nxc-auto.sh git:(main) ✗

这段输出非常有价值！`nxc` (NetExec) 帮您一次性确认了目标的关键身份信息。

以下是针对这条输出的详细中文解读，以及它对您后续渗透测试的影响：

### 1. 关键情报解读

|字段|值|含义与分析|
|---|---|---|
|**IP & Port**|`10.64.182.102 445`|目标开放了 SMB 服务，这是 Windows 域环境的核心入口。|
|**Hostname**|`name:ATTACKTIVEDIREC`|**计算机名**是 `ATTACKTIVEDIREC`。|
|**Domain (AD)**|`domain:spookysec.local`|**AD 域名 (DNS)** 是 `spookysec.local`。**这是您后续所有操作的基准。**|
|**OS**|`Windows 10 / Server 2019`|操作系统较新。这意味着默认安全性较高（例如禁用了 SMBv1，Defender 可能开启）。|
|**Signing**|`signing:True`|**非常重要！** SMB 签名已开启。这通常意味着**这台机器是域控制器 (DC)**，或者是一台被加固的服务器。|

---

### 2. 回答您之前的问题：两个名字分别是什么？

根据这条输出，我们可以直接得出结论：

- **Active Directory 域名 (DNS Name):**
    
    - **`spookysec.local`**
        
    - (这是您在使用 LDAP, Kerberos, 或 `user@domain` 登录时要用的)
        
- **NetBIOS 域名 (推测):**
    
    - 通常是域名的第一部分大写，即 **`SPOOKYSEC`**。
        
    - (这是您在使用 `SPOOKYSEC\user` 格式登录时用的)
        

---

### 3. `signing:True` 意味着什么？ (攻击者视角)

这是一个“坏消息”，也是一个“好消息”。

- **坏消息 (Bad News):** 您**不能**对这台机器进行 **NTLM Relay (中继攻击)**。如果您捕获了其他机器发来的哈希，想把它们“弹”到这台机器上获取 Shell，会因为签名验证失败而拒绝。
    
- **好消息 (Good News):** 只有域控制器 (DC) 默认强制开启 SMB 签名。这进一步证实了 **`10.64.182.102` 就是域控制器**。擒贼先擒王，您直接找到了核心目标。
    

### 4. 接下来该做什么？ (Next Steps)

既然知道了域名和 IP，您的第一步必须是**配置本地 DNS**，否则很多工具会报错。

#### **第一步：修改 `/etc/hosts`**

攻击机无法解析 `spookysec.local`，您需要手动告诉它。

Bash

```
# 在您的 Kali/攻击机上运行：
sudo echo "10.64.182.102 spookysec.local attacktivedirec.spookysec.local" >> /etc/hosts
```

#### **第二步：尝试枚举 (Enumeration)**

既然是新的目标，先看看有没有“未上锁的门”（空会话或默认密码）。

1. **检查空会话 (Null Session):** 看看能不能不输密码读取共享或用户列表。
    
    Bash
    
    ```
    nxc smb 10.64.182.102 -u '' -p '' --shares
    # 或者
    nxc smb 10.64.182.102 -u 'guest' -p '' --shares
    ```
    
2. **枚举用户 (RID Cycling):** 如果空会话允许，尝试“猜”出域内的用户列表。
    
    Bash
    
    ```
    nxc smb 10.64.182.102 -u '' -p '' --rid-brute
    ```


echo "10.64.182.102 spookysec.local attacktivedirec.spookysec.local spookysec attacktivedirec" | sudo tee -a /etc/hosts  

add all :  hostname, short domian, long domain,  and hostname+long name  to etc hosts

