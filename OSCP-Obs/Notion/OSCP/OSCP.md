python3 -c 'import pty; pty.spawn("/bin/bash")'

Ctrl + Z

stty raw -echo && fg

reset

screen

export TERM=xterm

clear

![[Screenshot_2025-08-26_at_16.55.12.png]]

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

[[135]]

[[AD Roadmap]]

### 渗透步骤说明（中文）

1. 上传钓鱼触发文件并获取目标机器的 shell。
2. 使用 **PrintSpoofer** 提权（和实验、PDF 第 474-478 页相同的方法）。
3. 禁用所有防御型杀软，和实验里类似。
4. 使用 **BloodHound 或 PowerView** 枚举域信息。
5. 使用 harmj0y 提供的委派攻击方法来利用委派漏洞。
6. 找到某台机器上的 ssh key，破解后登录到 ansible 机器。
7. 使用 `sudo` 提权。
8. 切换到 `ansiblesvc` 用户，并用 home 目录中的 ssh key 登录。
9. 使用 `sudo` 再次提权。
10. 找到属于 dave 用户的 **ccache ticket**，并传回到攻击机。
11. 在环境变量中设置该票据。
12. 在 BloodHound 中查看 dave 用户在哪台机器上有会话，然后使用 Kerberos 认证连接那台机器。
13. 创建 SQL 二进制（和课程里第 575-579 页的方法一样），并执行以下命令来利用 SQL 链接：

```Plain
EXECUTE AS LOGIN = 'su';
EXEC ('sp_configure ''show options'', 1; reconfigure') AT SQL0
EXEC ('xp_cmdshell ''hostname && whoami'' ') AT SQL0

```

---

### 如果已经拿到命令执行权限

- 可以用任意方式上线（如：`powershell.exe -e encodedHere`）。
- 然后继续：

1. 再次用 **PrintSpoofer** 提权（PDF 第 474-478 页方法）。
2. 禁用杀软，使用 **mimikatz** dump 哈希，找到 zen 用户的密码。
3. 在 BloodHound 中确认该用户是否能读取 LAPS 密码。
4. 读取 LAPS 密码（用任何合适方法），针对域内留下的机器（需 BloodHound 确认）。
5. 在 SQL 机器上 **Pass-the-Hash** 并启用 RDP，然后以管理员身份登录。
6. 从这台 RDP 机器，再 RDP 到存有 LAPS 密码的那台机器，并使用 LAPS 密码登录。
7. 禁用所有防御（因为有防火墙规则阻挡，需要移除才能从 Kali 连接目标）。
8. 在 SQL 机器上用 **msf shell 设置 socks proxy**（OSEP PDF 第 511-512 页方法）。
9. 从 Kali 连接到目标机器（即之前确定的那台机器）。
10. 获取 `secret.txt` 文件。

  

[[WIN PE]]