how to get dotnet version

下面给你 **CMD** 和 **PowerShell (pwsh)** 两种方式查询 Windows 主机上的 **.NET Framework** 与 **.NET Core / .NET 5+ / .NET Runtime** 版本（渗透测试中常用）。

---

# ✅ **CMD 获取 .NET Framework 版本**

### 1. 读取注册表（最常用）

```
reg query "HKLM\SOFTWARE\Microsoft\NET Framework Setup\NDP" /s

```

### 2. 只显示安装的最高版本（快速）

`reg query "HKLM\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release`

Release 值对应版本：

|Release 值|.NET Framework|
|---|---|
|528049|4.8|
|461808|4.7.2|
|461308|4.7.1|
|460798|4.7|

---

# ✅ **CMD 获取 .NET Core / .NET 5+ 版本**

`dotnet --list-runtimes dotnet --list-sdks`

如果 dotnet 不在 PATH，可以查默认安装路径：

`dir "C:\Program Files\dotnet" dir "C:\Program Files (x86)\dotnet"`

---

# ✅ **PowerShell 获取 .NET Framework 版本**

### 1. 读取注册表（最标准）

`Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse |     Get-ItemProperty -Name Version,Release -ErrorAction SilentlyContinue |     Where-Object { $_.PSChildName -match '^(?!S)\p{L}'} |     Select-Object PSChildName, Version, Release`

### 2. 快速获取 .NET Framework 4.x 版本

`(Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full').Release`

### 3. 获取对应的实际版本：

`$release = (Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full').Release Switch ($release) {     {$_ -ge 528049} {'4.8'}     {$_ -ge 461808} {'4.7.2'}     {$_ -ge 461308} {'4.7.1'}     {$_ -ge 460798} {'4.7'} }`

---

# ✅ **PowerShell 获取 .NET Core / .NET 5+ 版本**

### 如果 dotnet 在 PATH：

`dotnet --list-runtimes dotnet --list-sdks`

### 若 dotnet 不在 PATH：

`Get-ChildItem "C:\Program Files\dotnet\shared\" -Directory Get-ChildItem "C:\Program Files (x86)\dotnet\shared\" -Directory`

---

# 🔥 场景：如果你是在 Evil-WinRM / PSRemoting 中

这些命令都可以直接运行：

CMD 版本：

`cmd /c reg query "HKLM\SOFTWARE\Microsoft\NET Framework Setup\NDP" /s cmd /c dotnet --list-runtimes`

PowerShell 版本：

`Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse dotnet --list-runtimes`

