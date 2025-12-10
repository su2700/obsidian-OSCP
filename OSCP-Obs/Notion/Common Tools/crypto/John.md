
```
john --format=krb5tgs --wordlist=/usr/share/wordlists/rockyou.txt hashes.kerberoast

```



┌──(kali㉿kali)-[~/oscpb]
└─$ cat MS01   
web_svc::OSCP:aaaaaaaaaaaaaaaa:bfd1e69873c72b1f68c73a8673c127f0:010100000000000000153d6d5342db010feb495e8a000888000000000100100045004800790063004200530069004b000300100045004800790063004200530069004b000200100064006700630059004c007000640053000400100064006700630059004c007000640053000700080000153d6d5342db0106000400020000000800300030000000000000000000000000300000fe5d504c831d09e971c2780dc7ed5359043fcd2c79c9474d8535d93f2fa1205a0a001000000000000000000000000000000000000900260063006900660073002f003100390032002e003100360038002e00340035002e003200300033000000000000000000
                                                                                                                                                                            
┌──(kali㉿kali)-[~/oscpb]
└─$ john -wordlist=/usr/share/wordlists/rockyou.txt MS01
Using default input encoding: UTF-8
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Diamond1         (web_svc)     
1g 0:00:00:00 DONE (2024-11-29 12:45) 25.00g/s 614400p/s 614400c/s 614400C/s travon..280789
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
Session completed. 


```
john --list=formats

```
|用途|JtR 格式名称|Hashcat 对应|
|---|---|---|
|NTLM|`nt`|`1000`|
|LM|`lm`|`3000`|
|NetNTLMv1|`netntlm`|`5500`|
|NetNTLMv2|`netntlmv2`|`5600`|
|Kerberos TGS etype 23|`krb5tgs`|`13100`|
|Kerberos AS-REP|`krb5asrep`|`7500`|
|bcrypt|`bcrypt`|`3200`|
|SHA512 Unix|`sha512crypt`|`1800`|
|SHA256 Unix|`sha256crypt`|`7400`|


# John the Ripper 能自动判断哈希类型吗？

**可以，但不完全可靠。**

John the Ripper（JtR）具有一种 **自动格式识别能力**，会根据哈希的：

- 字符结构
    
- 前缀
    
- 长度
    
- 已知模式
    

来尝试判断是哪种哈希类型。

但是，这个识别机制 **只在部分哈希上表现良好**，并不能像 Hashcat 模块那样精确。

---

# 🧩 ✅ JtR 自动识别效果好的情况

John 能准确识别以下带 **标准前缀** 或 **结构明显** 的哈希：

|哈希类型|示例|自动识别效果|
|---|---|---|
|Unix 系列（/etc/shadow）|`$6$...` `$5$...` `$1$...`|✔️ 很准确|
|bcrypt|`$2a$10$...`|✔️ 非常准确|
|sha512crypt|`$6$...`|✔️ 很好|
|sha256crypt|`$5$...`|✔️ 很好|
|descrypt|13 个字符的传统 DES|✔️ 成功率高|

这些格式都有固定前缀，因此 John 能轻松识别。

---

# 🧩 ❌ JtR 自动识别效果差的情况

以下哈希类型 **不适合依赖自动判断**：

|哈希类型|说明|自动识别|
|---|---|---|
|MD5 (raw)|无前缀|❌ 不可靠|
|SHA1 / SHA256 (raw)|无前缀|❌ 经常误判|
|NTLM|常被误认为 raw-md4|❌ 几乎不准|
|NetNTLMv1/v2||❌ 不能自动识别|
|Kerberos (AS-REP / TGS-REP)|etype 17/18/23|❌ 必须手动指定|
|各类应用程序自定义格式||❌ 无法识别|

原因是很多 "无前缀" 的哈希看起来很相似（都是 hex 串）。

---

# 📌 如何使用自动识别？

你可以直接运行：

`john hash.txt`

如果 John 能确定格式，它会自动选择。  
如果不行，它会报错或尝试使用错误的格式。

---

# 📌 手动识别更安全

强烈建议对以下类型 **手动指定格式**：

- NTLM → `--format=nt`
    
- NetNTLMv2 → `--format=netntlmv2`
    
- Kerberos TGS → `--format=krb5tgs`
    
- Kerberos AS-REP → `--format=krb5asrep`
    
- raw SHA1 → `--format=raw-sha1`
    
- raw MD5 → `--format=raw-md5`


 john --format=Raw-MD5 hash --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
Warning: no OpenMP support for this hash type, consider --fork=8
Press 'q' or Ctrl-C to abort, almost any other key for status
December31       (?)
1g 0:00:00:00 DONE (2025-11-16 21:36) 14.28g/s 7745Kp/s 7745Kc/s 7745KC/s FOREVER15..DINDIN
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed.
➜  153 john --show --format=Raw-MD5
Password files required, but none specified
➜  153 john --show --format=Raw-MD5 hash
?:December31

1 password hash cracked, 0 left
➜  153