
how to create a symbolic link 

# 符号链接必须放在 root 可执行的 PATH 中

当你运行：

`sudo incursore`

sudo 不会使用你的用户 PATH，而是使用 **root 的 PATH**。

通常 root 的 PATH 包含：

`/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin`

因此，只要你的符号链接放在以下目录之一，就可以：

- `/usr/local/bin/`（最推荐）
    
- `/usr/bin/`
    
- `/bin/`
    

如果你是按照我之前的建议放在 `/usr/local/bin`：

`sudo ln -s /home/noah/Documents/incursore/incursore.sh /usr/local/bin/incursore`

那么 root 也能找到它。

---

# 🧩 条件 2：脚本本身必须有执行权限

无论是否使用 root，脚本必须有 `+x` 权限：

`chmod +x /home/noah/Documents/incursore/incursore.sh`

---

# ✔️ 完成这两个条件后，你可以运行：

普通用户运行：

`incursore`

root 运行：

`sudo incursore`

都会成功执行。

---

# 🧩 如何检查 root 的 PATH？

你可以查看：

`sudo env | grep PATH`

如果里面包含 `/usr/local/bin`，那么 `sudo incursore` 100% 能运行。