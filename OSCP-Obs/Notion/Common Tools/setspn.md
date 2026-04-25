```
 setspn -T medin -Q ​ */*
```

```
setspn -Q */*

```

## 、先说结论（重点）

1. **`medin` 不是当前域** → LDAP 连接失败是正常的
    
2. `setspn` **自动回退到当前域 `corp.local`**
    
3. **你已经成功枚举出可 Kerberoast 的账户：`fela`**
    
4. 输出结果 **100% 证明你之前 Kerberoasting 的路线是对的**
    

---

## 二、为什么会有这个 LDAP Error？（不是你的错）

`Ldap Error(0x51 -- Server Down): ldap_connect Failed to retrieve DN for domain "medin" Warning: No valid targets specified, reverting to current domain.`

### 含义是：

- `-T medin`：你**指定了一个不存在 / 无法解析的域**
    
- 当前机器 **不在 `medin` 域**
    
- DNS / LDAP 找不到 `medin` 的 DC
    

👉 所以 `setspn` 做了一个**非常关键的自动行为**：

`reverting to current domain`

也就是：

> **改用当前登录的域：`corp.local`**

---

## 三、当前真实环境（从输出中可以确认）

### 域信息

`DC=corp,DC=local`

👉 当前域：**`corp.local`**

### 域控

`CN=OMEGA,OU=Domain Controllers,DC=corp,DC=local`

👉 域控主机名：**OMEGA**  
👉 FQDN：**omega.corp.local**

---

## 四、哪些 SPN 是“有用的”？（这是核心）

### ❌ 无法 Kerberoast（系统账户）

`CN=OMEGA,OU=Domain Controllers CN=krbtgt,CN=Users`

- 域控 / krbtgt
    
- 不作为 Kerberoasting 目标
    

---

### ✅ **关键目标（你要的）**

`CN=fela,CN=Users,DC=corp,DC=local         HTTP/fela         HOST/fela@corp.local         HTTP/fela@corp.local`

#### 为什么 `fela` 非常重要？

- 👤 **普通域用户**
    
- 🔐 绑定了 **HTTP SPN**
    
- 🎯 可以请求 **TGS**
    
- 💣 **可以 Kerberoasting**

## 一、setspn 是干什么的？

`setspn` 是 **Windows 自带工具**，用于：

> **查询 / 管理 Active Directory 中的 SPN（Service Principal Name）**

在渗透测试中，它最常见的用途是：

👉 **枚举可用于 Kerberoasting 的服务账户**

---

## 二、参数逐个解释（重点）

### 🔹 `-T medin`

`-T <DomainName>`

- 指定 **要查询的域**
    
- 这里的：
    
    `medin`
    
    就是 **目标 Active Directory 域名**
    

等价于：

> “在 **medin 域** 中进行查询”

---

### 🔹 `-Q */*`

`-Q <query>`

- `-Q` = **Query（查询）**
    
- `*/*` 的含义是：
    

|部分|含义|
|---|---|
|`*`|任意服务名|
|`/`|SPN 分隔符|
|`*`|任意主机名|

📌 **`*/*` = 查询域中所有 SPN**

---

## 三、整条命令的含义（一句话版）

> **在 medin 域中，查询所有注册了 SPN 的账户**