check nmap vuln if no found 
check msf

from nmap, it is a windows 2008, such old , should have some vuln 

get domain

```JavaScript
nbtscan 192.168.236.40     
NBT stands for NetBIOS over TCP/IP.

nslookup
dig -x
echo "192.168.236.40 INTERNAL" | sudo tee -a /etc/hosts


┌──(kali㉿hk-DC01)-[~]
└─$ dig @$IP axfr internal
AXFR stands for Authoritative Zone Transfer. find hidden domain

responder， mitm6 and impacket-ntlmrelayx:
if nmap shows : Message signing enabled but not required

The tool impacket-ntlmrelayx can be used in this scenario because the SMB server has message signing enabled but not required, which creates a potential vulnerability that can be exploited using NTLM relay attacks.

Why This Works:
NTLM Relay Attack:

NTLM relay is a type of attack where an attacker intercepts NTLM authentication requests and relays them to another server to authenticate as the victim.
If SMB message signing is not required, the attacker can relay the intercepted NTLM credentials to the target SMB server and perform actions like creating files, executing commands, or dumping sensitive data.
Message Signing Not Required:

SMB message signing ensures the integrity of SMB communications. If it is not required, the attacker can modify or relay SMB traffic without being detected.
In this case, the server allows unsigned SMB connections, making it vulnerable to NTLM relay attacks.
impacket-ntlmrelayx:

This tool is part of the Impacket suite and is specifically designed to perform NTLM relay attacks.
It listens for incoming NTLM authentication requests and relays them to a target server (e.g., the SMB server in this case) to gain unauthorized access.
```

```JavaScript
smbclient -N -L \\\\192.168.236.40 
smbclient -N -L 192.168.236.40
```

```PowerShell
nmap -T4 -p445 --script "smb-vuln*" 192.168.221.40
```



pg_ctl: could not start server

pg_ctl -D ~/.msf4/db start

msfdb reinit

pip2 install pysmb