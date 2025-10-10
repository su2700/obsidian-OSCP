ps1 ans ps2 are module, need impordt first

```
PS: C:\xampp\htdocs\tmp> Import-Module .\PrivescCheck.ps1

PS: C:\xampp\htdocs\tmp> Invoke-PrivescCheck -Extended
```

 for MySQL if SSL is required
 
 pivoting git:(main) ✗  mysql -u root -h 127.0.0.1
ERROR 2026 (HY000): TLS/SSL error: SSL is required, but the server does not support it
➜  pivoting git:(main) ✗ mysql -u root -h 127.0.0.1 --ssl-mode=DISABLED

mysql: unknown variable 'ssl-mode=DISABLED'
➜  pivoting git:(main) ✗ mysql -u root -h 127.0.0.1 --ssl-mode=PREFERRED

mysql: unknown variable 'ssl-mode=PREFERRED'
➜  pivoting git:(main) ✗ mysql -u root -h 127.0.0.1 --skip-ssl
[[RunasCs]]

Welcome to the MariaDB monitor.  Commands end with ; or \g.


is used to display detailed information about **active network connections** and **listening ports** on your system.

since we find a db, so we check if a db is running, mysql 3306

[[netstat]]

```
netstat -ano

```
  
[[evil-winrm]]


To use **Evil-WinRM** (a tool for connecting to Windows Remote Management services), the following **ports must be open** on the target Windows machine:

---

### **Required Ports**

1. **WinRM (HTTP)** → **Port 5985** (Default for unencrypted HTTP traffic).
2. **WinRM (HTTPS)** → **Port 5986** (Default for encrypted HTTPS traffic).

- If the target uses **HTTPS (5986)**, add `**S**`:
    
    ```Plain
    bashCopy
    evil-winrm -i <TARGET_IP> -u <USERNAME> -p <PASSWORD> -S
    ```
    

```Shell
smbclient //$IP/WebApp -U thecybergeek%winniethepooh
```

```Plain
cmd /c sc qc Mysql

```

breaks down as follows:

- `**cmd /c**`: This tells Windows to open a new command prompt, run the command that follows, and then terminate the command prompt.
- `**sc**`: This is the Service Control command-line utility in Windows, used to communicate with the Service Control Manager and services.
- `**qc**`: This is a subcommand of `**sc**` that stands for **Query Configuration**. It retrieves and displays the configuration information of a specified Windows service.
- `**Mysql**`: This is the name of the service you want to query. In your case, it’s the MySQL service installed by XAMPP.