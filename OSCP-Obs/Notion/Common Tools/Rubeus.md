

```
.\Rubeus.exe kerberoast /outfile:hashes.kerberoast
```
- `.\Rubeus.exe`：在 Windows 下运行名为 **Rubeus** 的可执行文件（Rubeus 是一个与 Kerberos 协议交互的工具集，常被用来做合法的红队测试或恶意滥用）。
    
- `kerberoast`：这是 Rubeus 的一个子命令，意思是**对域内有注册 SPN（Service Principal Name，服务主体名）的服务请求 Kerberos 服务票据（TGS）**并导出这些票据中被服务密钥加密的部分。导出的内容通常包含可用于离线暴力破解/破解服务账号密码哈希的信息（即“kerberoast”攻击的核心数据）。
    
- `/outfile:hashes.kerberoast`：把导出的票据/哈希数据**写入到名为 `hashes.kerberoast` 的文件**中，方便后续分析或（在攻击场景下）离线破解。

```
sudo hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```






好的，中文解释如下 — 核心在于 **会话类型与认证环境的差异**，不是 本身有 bug：

### 结论（简短）

通过 **Evil-WinRM** 登录时 Rubeus 的 LDAP/DirectoryServices 枚举会失败，常见原因是 WinRM 远程会话**没有完整的本地交互登录环境 / Kerberos 凭证 / 二跳权限**，导致 .NET 的 `System.DirectoryServices` 无法正常 bind 到域控制器（出现你看到的 `DirectoryServicesCOMException`）。而通过 **SSH**（或直接交互式登录）通常会产生一个完整的登录会话，加载用户配置和票证，因而 Rubeus 可以正常工作。

---

### 为什么 Evil-WinRM 会失败 — 常见具体原因

1. **“第二跳”/凭证委派问题（CredSSP / Kerberos delegation）**  
    WinRM 默认会话使用的身份认证（通常是 NTLM）并不会把你的 Kerberos TGT 或委派权限传递给另一个目标（DC）。DirectoryServices 在内部可能需要使用 Kerberos/ADSI 去查询 DC，结果被二跳限制挡住。
    
2. **没有可用的 Kerberos TGT（或票证存在性不同）**  
    在交互式登录下会生成并缓存 TGT（`klist` 可见）。WinRM 会话经常没有 TGT（或是被限制），因此基于 Kerberos 的域枚举失败。
    
3. **会话类型 / 用户配置未完整加载**  
    WinRM 是服务创建的远程会话，可能不会加载完整的用户配置文件或某些 COM/ADS 库在此环境下不可用，导致 `System.DirectoryServices.DirectoryEntry.Bind()` 失败。
    
4. **网络/权限“从远程会话到DC”的访问被限制**  
    有时域对端策略或防火墙/ACL 阻止来自管理通道（如 WinRM 会话）的 LDAP 访问。交互式/SSH 会话可能绕过这类限制（或以不同身份运行）。
    
5. **.NET/COM 在 WinRM 环境的限制**  
    部分环境下远程 PowerShell/WinRM 无法加载或访问某些本地 COM/ADS 接口，会触发 DirectoryServices 异常。