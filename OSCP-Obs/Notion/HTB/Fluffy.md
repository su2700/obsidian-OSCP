
account: j.fleischman / J0elTHEM4n1990!

┌──(noah㉿noah)-[~/HTB/forest]
└─$ netexec ldap dc01.fluffy.htb -u j.fleischman -p 'J0elTHEM4n1990!' -M adcs
LDAP        10.10.11.69     389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:fluffy.htb)
LDAP        10.10.11.69     389    DC01             [+] fluffy.htb\j.fleischman:J0elTHEM4n1990!
ADCS        10.10.11.69     389    DC01             [*] Starting LDAP search with search filter '(objectClass=pKIEnrollmentService)'
ADCS        10.10.11.69     389    DC01             Found PKI Enrollment Server: DC01.fluffy.htb
ADCS        10.10.11.69     389    DC01             Found CN: fluffy-DC01-CA

### netexec `-M` 参数

- `-M` 或 `--module` 用于指定模块（module），告诉 netexec 要执行哪种任务。
    
- netexec 内置多个模块，比如：
    
    - `adcs`：用于 **Active Directory Certificate Services (AD CS) 枚举和攻击**
        
    - `ldap`：普通 LDAP 枚举
        
    - `group` / `user` / `domain` 等模块：用于获取用户、组、域信息

你这条 `netexec ldap ... -M adcs` 输出已经告诉我们 DC 上 **确实存在证书颁发机构 (CA)**。具体分析如下：

---

### 1️⃣ 通过 LDAP 查询到 CA 服务

`ADCS        10.10.11.69     389    DC01             [*] Starting LDAP search with search filter '(objectClass=pKIEnrollmentService)' ADCS        10.10.11.69     389    DC01             Found PKI Enrollment Server: DC01.fluffy.htb ADCS        10.10.11.69     389    DC01             Found CN: fluffy-DC01-CA`

- `(objectClass=pKIEnrollmentService)`  
    这是 Active Directory 中的一个对象类，用于 **表示注册/颁发证书的服务**（即 AD CS）。  
    如果 LDAP 查询返回结果，说明 DC 上有配置 AD CS（Active Directory Certificate Services）。
    
- `Found CN: fluffy-DC01-CA`  
    CN 就是 **证书颁发机构的名称**。这明确说明：DC01 上存在名为 `fluffy-DC01-CA` 的 CA。

```
 certipy-ad find -u j.fleischman@fluffy.htb -p 'J0elTHEM4n1990!' -vulnerable -stdout
```

```
 bloodhound-ce-python -c all -d fluffy.htb -u j.fleischman -p 'J0elTHEM4n1990!' -ns 10.10.
11.69 --zip
```
在 BloodHound / CE-Python Collector 中，`-d` 参数 **只接受域名**（Domain Name），而不是域控制器（DC）的主机名。原因如下：

---

### 1️⃣ `-d` 是域名，不是 DC 名称

- `-d fluffy.htb` → 指定 **域名**，BloodHound 用它来：
    
    - 构建 LDAP 路径（`DC=fluffy,DC=htb`）
        
    - 枚举用户、组、计算机、ACL 等
        
- **不能写成 DC01,FLUFFY.HTB**：
    
    - BloodHound 不是直接针对某台 DC，而是针对整个域进行 LDAP 查询
        
    - 多个值会被解析失败，或者报错 `Invalid domain format`
        

---

### 2️⃣ 指定 DC 的方法

如果你想指定具体的域控制器 IP 或主机名，用的是 **`-ns` 或 `--server`**：

- `-ns 10.10.11.69` → 指定 DNS 或 LDAP 解析用的 DC/IP
    
- `--server DC01.fluffy.htb` → 强制连接到指定 DC
    

示例：

`bloodhound-ce-python -c all -d fluffy.htb -u j.fleischman -p 'J0elTHEM4n1990!' -ns 10.10.11.69`

- `-d fluffy.htb` → 域名
    
- `-ns 10.10.11.69` → 指定解析服务器/目标 DC
    
- 这样可以同时保证域查询正确和连接到指定 DC
    

---

### ✅ 总结

- `-d` → **域名**，用于 LDAP 构造路径
    
- `-ns` / `--server` → **DC 主机/IP**，用于实际连接
    
- 不要把 DC 名称和域名混在 `-d` 里，会导致参数解析错误
    

---
