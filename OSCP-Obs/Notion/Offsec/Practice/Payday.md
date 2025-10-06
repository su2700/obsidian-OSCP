name: 
admin/admin
John Doe
patrick

try all the name , and all the passwd use name

check version, amd? arm? 32? 64? 
use old version of linpeas or winpeas
use lingpeas.sh > lps, 

pass:


http://192.168.107.39/?version
to get version 


➜  payday ssh patrick@192.168.107.39
Unable to negotiate with 192.168.107.39 port 22: no matching host key type found. Their offer: ssh-rsa,ssh-dss
➜  payday 

That error means your client refused the server’s host key algorithms (the server only offers old types ssh-rsa / ssh-dss) while your OpenSSH client has those algorithms disabled by default for security. You can force the client to accept the legacy host key type (or fix it on the server). Pick one of these options.
```
ssh -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedKeyTypes=+ssh-rsa patrick@192.168.107.39

```
```
find / -xdev \( -perm -u=s -o -perm -g=s \) -exec ls -l {} \; 2>/dev/null

```