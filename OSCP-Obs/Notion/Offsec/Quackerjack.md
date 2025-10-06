Starting Nmap 7.95 ( [https://nmap.org](https://nmap.org/) ) at 2025-07-23 10:49 CEST  
Nmap scan report for 192.168.214.57  
Host is up (0.034s latency).  
Not shown: 65527 filtered tcp ports (no-response)  
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit  
PORT STATE SERVICE VERSION  
21/tcp open ftp vsftpd 3.0.2  
| ftp-syst:  
| STAT:  
| FTP server status:  
| Connected to ::ffff:192.168.45.223  
| Logged in as ftp  
| TYPE: ASCII  
| No session bandwidth limit  
| Session timeout in seconds is 300  
| Control connection is plain text  
| Data connections will be plain text  
| At session startup, client count was 3  
| vsFTPd 3.0.2 - secure, fast, stable  
|_End of status  
| ftp-anon: Anonymous FTP login allowed (FTP code 230)  
|_Can't get directory listing: TIMEOUT_  
_22/tcp open ssh OpenSSH 7.4 (protocol 2.0)_  
_| ssh-hostkey:_  
_| 2048 a2:ec:75:8d:86:9b:a3:0b:d3:b6:2f:64:04:f9:fd:25 (RSA)_  
_| 256 b6:d2:fd:bb:08:9a:35:02:7b:33:e3:72:5d:dc:64:82 (ECDSA)_  
_|_ 256 08:95:d6:60:52:17:3d:03:e4:7d:90:fd:b2:ed:44:86 (ED25519)  
80/tcp open http Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16)  
|_http-title: Apache HTTP Server Test Page powered by CentOS_  
_|http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16_  
_| http-methods:_  
_| Potentially risky methods: TRACE_  
_111/tcp open rpcbind 2-4 (RPC #100000)_  
_| rpcinfo:_  
_| program version port/proto service_  
_| 100000 2,3,4 111/tcp rpcbind_  
_| 100000 2,3,4 111/udp rpcbind_  
_| 100000 3,4 111/tcp6 rpcbind_  
_|_ 100000 3,4 111/udp6 rpcbind  
139/tcp open netbios-ssn Samba smbd 3.X - 4.X (workgroup: SAMBA)  
445/tcp open netbios-ssn Samba smbd 4.10.4 (workgroup: SAMBA)  
3306/tcp open mysql MariaDB 10.3.23 or earlier (unauthorized)  
8081/tcp open http Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16)  
|_http-title: 400 Bad Request  
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16  
Service Info: Host: QUACKERJACK; OS: Unix

Host script results:  
| smb2-security-mode:  
| 3:1:1:  
|_ Message signing enabled but not required  
| smb-security-mode:  
| account_used: guest  
| authentication_level: user  
| challenge_response: supported  
|_ message_signing: disabled (dangerous, but default)  
| smb-os-discovery:  
| OS: Windows 6.1 (Samba 4.10.4)  
| Computer name: quackerjack  
| NetBIOS computer name: QUACKERJACK\x00  
| Domain name: \x00  
| FQDN: quackerjack  
|_ System time: 2025-07-23T04:52:41-04:00  
| smb2-time:  
| date: 2025-07-23T08:52:40  
|_ start_date: N/A  
|_clock-skew: mean: 1h19m59s, deviation: 2h18m36s, median: -2s

Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .  
Nmap done: 1 IP address (1 host up) scanned in 214.26 seconds

  

```PowerShell
sudo nmap -Pn -n $IP -sC -sV -p- --open
```

-Pn: skip host discovery, P, ping, control host discovery , n, no

-n : no DNS

-sV: scan type, version

-sC: script, run default scripts

```PowerShell
sudo nmap -Pn -n $IP -sU --top-ports=100 --reason
```

```PowerShell
sudo gobuster dir -w '/home/kali/Desktop/wordlists/dirbuster/directory-list-2.3-medium.txt' -u http://$IP:80 -t 42 -b 400,401,403,404 --no-error 
```

Port 111 — RPCBind

It’s strange to me to see this on a Linux computer as this normally applies to Windows. For Windows machine I use the below command to enumerate RPC.

```Plain
rpcclient -U '' -N $IP
```

Port 139,445 — SMB and/or Remote Management

My routine for this combination is as follows:

```Plain
enum4linux $IP

smbclient -N -L \\\\$IP\\
```

in SSL/TLS cert, **Common Name** is the **fully qualified domain name**