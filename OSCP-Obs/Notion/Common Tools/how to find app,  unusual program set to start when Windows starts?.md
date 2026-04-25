
```
wmic startup get caption, command
```

```
Get-CimInstance Win32_StartupCommand | Select-Object Name, Command, Location
```
### 1. `Get-CimInstance Win32_StartupCommand`

这是命令的“抓取”部分。

- **`Get-CimInstance`**: 这是一个现代的 PowerShell 指令，用于获取系统的硬件或软件信息（基于 CIM 标准，是旧版 `Get-WmiObject` 的升级版）。
    
- **`Win32_StartupCommand`**: 这是一个特定的“系统信息类”，它专门记录 Windows 中所有启动项的信息。它会去扫描注册表（Run 键值）、启动文件夹（Startup folders）等多个地方。
    

### 2. `|` (管道符号)

这个符号的作用是“传递”。它把第一步抓取到的**所有**原始数据（包含几十个隐藏属性），传递给下一步进行筛选。

### 3. `Select-Object Name, Command, Location`

这是命令的“筛选”部分，决定了你在屏幕上看到的内容：

- **`Name`**: 显示启动项的**名称**（例如：`OneDrive` 或 `backdoor`）。
    
- **`Command`**: 显示启动项运行的**具体命令或路径**（例如：`C:\Windows\system32\notepad.exe`）。这对于发现恶意程序非常关键。
    
- **`Location`**: 显示这个启动项是**从哪里加载的**（例如：它是存在于注册表的 `HKLM` 还是用户的 `Startup` 文件夹）。

这条命令之所以能成功找到隐藏的“后门”程序，是因为它利用了 Windows 底层的**管理基础架构**，绕过了普通用户界面的限制。

简单来说，它的工作原理可以分为“**情报来源**”、“**处理方式**”和“**展示逻辑**”三个层面：

### 1. 情报来源：Win32_StartupCommand

Windows 系统中，一个程序想要“自启动”，通常会藏在很多不同的地方，比如：

- **注册表 (Registry)**：有好几个不同的路径（如 `Run` 和 `RunOnce`）。
    
- **启动文件夹 (Startup Folders)**：包括个人用户文件夹和所有用户通用的系统文件夹。
    

如果你手动去找，需要翻遍五六个文件夹和注册表路径。而 `Win32_StartupCommand` 是一个**预设的系统查询类**，它就像一个“侦察兵”，会自动扫描上述所有位置，并把结果汇总在一起。

### 2. 核心技术：CIM (公共信息模型)

`Get-CimInstance` 是 PowerShell 调用系统数据的“翻译官”。

- 它不直接读取文件，而是询问 Windows 的 **WMI/CIM 存储库**（这是一个记录了系统所有硬件和软件状态的庞大数据库）。
    
- 即使黑客把程序藏在很深的临时目录（如 `Temp`），只要这个程序被注册为启动项，数据库里就一定会有记录。
    

### 3. 处理方式：管道符 (Pipeline `|`)

这是 PowerShell 的精髓。

- `Get-CimInstance` 抓取到的原始数据非常多且乱（包括用户 SID、设置 ID 等你不需要的信息）。
    
- 通过 `|` 符号，就像把这些杂乱的数据丢进了一条**传送带**，传给下一个环节进行精简。
    

### 4. 展示逻辑：Select-Object

这是最后一道“过滤器”。

- 它从传送带上只挑选出你最关心的三样东西：**名字 (Name)**、**命令路径 (Command)** 和 **所在位置 (Location)**。
    
- **Command** 是最关键的：正常的程序路径通常在 `C:\Program Files`，而这个命令揭示了 `backdoor.exe` 躲在 `Temp` 文件夹里，瞬间让它原形毕露。
    

---

### 总结

这条命令之所以有效，是因为它**把散落在系统各处的启动痕迹一网打尽**，并用最直观的方式对比给你看。