# tar wildcard 提权攻击原理（核心解释）

整个攻击成立，是因为 **tar 命令 + wildcard（*） + root 运行** 产生了一个漏洞链。

攻击原理可以分成 **3 个关键点**：

---

# ⭐ **原理 1：通配符 * 会被 Bash 展开为文件名**

在 Bash 中，脚本里写的：

`tar -zcf backup.tgz *`

不会直接传给 tar。

Bash 会先把 `*` 替换成当前目录所有文件名：

`tar -zcf backup.tgz file1 file2 image.png aaa.txt ...`

**所以只要攻击者能在目录里创建文件，就能控制这条命令的参数！**

---

# ⭐ **原理 2：tar 的 “危险参数” 可以执行系统命令**

tar 有一个特殊功能：

`--checkpoint-action=exec=<命令>`

当 tar 处理到某个点，会执行这个命令。

例如：

`tar -zcf out.tgz file --checkpoint=1 --checkpoint-action=exec=whoami`

tar 会执行：

`whoami`

📌 **这意味只要 tar 看到这个参数，就能执行任意命令**。

---

# ⭐ **原理 3：攻击者可以创建“伪装成参数的文件名”**

如果攻击者创建文件名：

`--checkpoint=1 --checkpoint-action=exec=sh privesc.sh`

这些文件会被 * 自动包含进去。

Bash 展开后 tar 命令变成：

`tar -zcf backup.tgz --checkpoint=1 --checkpoint-action=exec=sh privesc.sh file1 file2 ...`

tar 会把这些“文件名”解析为 **参数**，不是文件！

于是 tar 执行：

`sh privesc.sh`

---

# ⭐ **原理 4：整个脚本是 root（或更高权限）运行**

如果这是一个 root 的 crontab：

`* * * * * root /path/compressToBackup.sh`

那么 tar 执行 privesc.sh 时，也是 **root 权限执行**。

攻击者就能：

- 修改 /etc/sudoers
    
- 写 suid 程序
    
- 写 /etc/passwd
    
- 获取 root shell
    

---

# 🎯 **一句话总结攻击原理（OSCP 考试版）**

> 由于 Bash 会将通配符 * 展开为所有文件名，攻击者可以创建文件名看起来像 tar 参数（如 --checkpoint-action=exec=sh），tar 会将其当作命令行参数解析，从而执行攻击者提供的脚本。如果 tar 是由 root 的定时任务运行，则脚本将以 root 权限执行，从而完成提权。