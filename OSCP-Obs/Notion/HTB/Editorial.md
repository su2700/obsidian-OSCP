[[80]]since 80 open, can use whatweb

since have photo, download, then use file and ls -liah, then exiftool

find some text, get the means

Since two domain name, so we check

```BNF
curl -I tiempoarriba.htb
HTTP/1.1 301 Moved Permanently
Server: nginx/1.18.0 (Ubuntu)
Date: Sun, 07 Sep 2025 08:05:54 GMT
Content-Type: text/html
Content-Length: 178
Connection: keep-alive
Location: http://editorial.htb
```

- `curl` = command-line tool to transfer data over HTTP, HTTPS, FTP, etc.
- `I` (capital i) = **fetch only the HTTP headers** (a "HEAD" request).

build http server, this is upload only

```BNF
sudo goshs -i 10.10.14.11 -p 80 -uo
sudo goshs -i 10.10.14.11 -p 80 
```

  

add host

```BNF
echo "10.10.11.20 editorial.htb" | sudo tee -a /etc/hosts
```

`tee` is a Linux command that **writes input both to a file and to standard output**.

- By default, `tee file` overwrites the file.
- With `a` (append), it **adds** the text to the end of the file instead of overwriting it.

Any request to `tiempoarriba.htb` returns **301 Moved Permanently** → redirecting you to `http://editorial.htb`

  

from [http://editorial.htb/upload](http://editorial.htb/upload), kick preview

➜ ~ nc -lvnp 80  
Connection from 10.10.11.20:44462  
GET / HTTP/1.1  
Host: 10.10.16.10  
User-Agent: python-requests/2.25.1  
Accept-Encoding: gzip, deflate  
Accept: _/_  
Connection: keep-alive

  

so , port is 44462 and it is a python server