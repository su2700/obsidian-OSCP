**Evil-WinRM** 本身并不直接将 **LDAP** 作为其核心传输协议（它主要使用 **WinRM/PSRP** 协议）。然而，在渗透测试和红队行动中，Evil-WinRM 经常与 LDAP 结合使用，或者通过它建立的远程会话来执行 LDAP 相关操作。

以下是它们之间的具体关系和用法：

### 1. 协议层面的区别

- **WinRM (Windows Remote Management):** Evil-WinRM 使用的基础。它运行在 **5985 (HTTP)** 或 **5986 (HTTPS)** 端口上，用于获取交互式 PowerShell 命令行环境。
    
- **LDAP (Lightweight Directory Access Protocol):** 运行在 **389 (LDAP)** 或 **636 (LDAPS)** 端口上，用于查询和管理 Active Directory (AD) 数据库中的对象（如用户、组、计算机）。
    

### 2. 如何在 Evil-WinRM 会话中使用 LDAP？

一旦你通过 Evil-WinRM 登录了目标机器，你可以利用该机器作为跳板，通过 PowerShell 与域控（DC）进行 LDAP 通信：

- **加载 AD 模块：** 如果你登录的机器安装了远程服务器管理工具 (RSAT)，你可以直接运行 LDAP 查询：
    
    PowerShell
    
    ```
    Import-Module ActiveDirectory
    Get-ADUser -Filter * -Properties Description
    ```
    
- **使用 PowerView 进行 LDAP 枚举：** 这是最常见的用法。你可以通过 Evil-WinRM 的 `upload` 功能上传 `PowerView.ps1`，然后在内存中加载它来执行 LDAP 查询：
    
    PowerShell
    
    ```
    # 在 Evil-WinRM 中
    *Evil-WinRM*> upload /path/to/PowerView.ps1
    *Evil-WinRM*> . ./PowerView.ps1
    *Evil-WinRM*> Get-DomainUser -LDAPFilter "(admincount=1)"
    ```
    

### 3. Evil-WinRM 对 LDAP 的间接依赖

- **Kerberos 认证 (`-r`, `--spn`):** 当你使用 Evil-WinRM 的 Kerberos 认证模式时，虽然通信是走 WinRM，但底层认证过程可能涉及到向域控（DC）请求票据，而 DC 验证身份的过程与 LDAP/AD 密切相关。
    
- **AD 资源访问：** 在通过 Evil-WinRM 进行横向移动时，攻击者通常会修改 LDAP 属性（如 `msDS-AllowedToDelegateTo`）来实现约束委派攻击。
    

---

### 总结

**Evil-WinRM 不会“自动”使用 LDAP**，但它是你**执行 LDAP 攻击**的一个远程入口。它提供了一个稳定的 Shell，让你能够运行复杂的脚本（如 PowerView、SharpHound）来通过 LDAP 协议探测整个内网。