

PORT STATE SERVICE VERSION  
21/tcp open ftp Microsoft ftpd  
80/tcp open http Microsoft IIS httpd 10.0  
|_http-server-header: Microsoft-IIS/10.0  
135/tcp open msrpc Microsoft Windows RPC  
139/tcp open netbios-ssn Microsoft Windows netbios-ssn  
445/tcp open microsoft-ds?  
5040/tcp open unknown  
9998/tcp open http Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)  
|_http-server-header: Microsoft-IIS/10.0  
17001/tcp open remoting MS .NET Remoting services  
49664/tcp open msrpc Microsoft Windows RPC  
49665/tcp open msrpc Microsoft Windows RPC  
49666/tcp open msrpc Microsoft Windows RPC  
49667/tcp open msrpc Microsoft Windows RPC  
49668/tcp open msrpc Microsoft Windows RPC  
49669/tcp open msrpc Microsoft Windows RPC

  

find CMS from nmap, then google CMS in GITHUB

[[21]]

one command get all from ftp

```SQL
wget -m --no-passive -b ftp://anonymous:@192.168.1.10/
wget -m --no-passive -P /path/to/download/directory ftp://anonymous:@192.168.1.10/
wget -m --no-passive -P /path/to/download/directory ftp://<ftp_user>:<ftp_password>@<ftp_server>/

```

  

  

80

  
[[135]]
135,593 Microsoft Remote Procedure Call

The RPC endpoint mapper can be accessed via TCP and UDP port 135, SMB on TCP 139 and 445 (with a null or authenticated session), and as a web service on TCP port 593.

  

[rpcdump.py](http://rpcdump.py/) -target-ip 192.168.240.65 192.168.240.65