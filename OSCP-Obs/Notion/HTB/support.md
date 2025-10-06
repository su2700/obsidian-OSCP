===========================================================
|    Domain Information via SMB session for 10.10.11.174    |
 ===========================================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found domain information via SMB
NetBIOS computer name: DC
NetBIOS domain name: SUPPORT
DNS domain: support.htb,  what need add to /etc/hosts
FQDN: dc.support.htb
Derived membership: domain member
Derived domain: SUPPORT

当你通过 SMB 枚举域控制器时，得到了以下信息：

- **NetBIOS 计算机名**：`DC`
    
- **NetBIOS 域名**：`SUPPORT`
    
- **DNS 域名**：`support.htb`
    
- **FQDN（完整域名）**：`dc.support.htb`
    

在渗透或实验环境中，很多工具（如 `smbclient`、`rpcclient`、`evil-winrm`、`impacket` 的一系列工具）需要通过 **域名或主机名** 来连接目标，而不仅仅是 IP。

但是在 HTB 这种环境里，一般不会有真实的 DNS 服务器帮你解析这些名字 → 所以你必须手动在 `/etc/hosts` 里建立 IP 与域名/主机名的对应关系。

---

所以 `/etc/hosts` 要写成这样：

`10.10.11.174   dc.support.htb dc support.htb DC SUPPORT`

这样做的原因是：

1. **FQDN** (`dc.support.htb`) → 很多工具会优先尝试完整域名来连接域控制器。
    
2. **域名** (`support.htb`) → 在认证时，Kerberos/NTLM 可能会用到域名。
    
3. **主机名** (`dc`) → 有些工具只会请求主机名。
    
4. **NetBIOS 名称** (`DC` / `SUPPORT`) → 有时 SMB 或 RPC 协议层面会直接使用。
    

---

总结：  
- IP：主机的实际地址（例如 `10.10.11.174`）。这是最终被 `/etc/hosts` 映射到的目标。
    
- FQDN：完整主机名（`dc.support.htb`），由主机名 + DNS 域名 组成。许多工具与认证流程会使用 FQDN。
    
- DNS domain（域名）：`support.htb`，常用于 Kerberos、LDAP 等协议里的域上下文。
    
- NetBIOS 名称：较旧的 SMB/NetBIOS 协议使用的短名（主机名 `DC`、域名 `SUPPORT`），有时工具或协议栈会直接用这些名字。

在 `/etc/hosts` 里把这些名字都映射到目标 IP，可以保证 **无论工具调用的是 FQDN、域名还是 NetBIOS 名称**，都能正确解析到目标域控的 IP，从而避免出错。
+----------------+        +---------------------------+        +----------------+
| IP                            |  --->   | FQDN                                      |  --->  | NetBIOS host   |
| 10.10.11.174   |               | dc.support.htb            |                             | DC             |
+----------------+        +---------------------------+        +----------------+
                             |
                             |---> DNS domain (域名)
                             |        support.htb
                             |
                             `---> NetBIOS domain (传统 SMB 域名)
                                      SUPPORT
![[Pasted image 20250920145203.png]]



从你提供的输出来看，你在尝试访问 SMB 分享 `//10.10.11.174/support-tools` 时遇到了几个不同问题。我来帮你分析每条命令的结果，并给一些解决方案。

---

### **1️⃣ 命令**

`smbclient  //10.10.11.174/support-tools  -U '' -N`

**输出：**

`tree connect failed: NT_STATUS_ACCESS_DENIED`

**分析：**

- 你尝试匿名访问 (`-U '' -N`)，但是 SMB 服务器要求凭证（用户名/密码），所以返回 **ACCESS_DENIED**。
    

---

### **2️⃣ 命令**

`smbclient  //10.10.11.174/support-tools   -N`

**输出：**

`Try "help" to get a list of possible commands.`

**分析：**

- 这条命令成功建立了连接，但可能只是建立了 SMB 句柄，没有权限列出目录内容。
    
- 有些 SMB 共享允许连接但不允许匿名浏览。
    

---

### **3️⃣ 命令**

`smbclient -L //10.10.11.174/support-tools  -U '' -N`

**输出：**

`Reconnecting with SMB1 for workgroup listing. do_connect: Connection to 10.10.11.174 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND) Unable to connect with SMB1 -- no workgroup available`

**分析：**

- 你在列共享 (`-L`) 时出错了。
    
- SMB1 尝试失败，提示 **资源不存在**。
    
- 很可能服务器使用 SMB2/3，而 `smbclient` 默认先尝试 SMB1 列共享。