❯ hashcat -m 1000 -a 0 web_svc.hash3 /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule

hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 3.1+debian  Linux, None+Asserts, RELOC, SPIR, LLVM 15.0.6, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
==================================================================================================================================================
* Device #1: pthread-haswell-Intel(R) Core(TM) i7-7700HQ CPU @ 2.80GHz, 14931/29927 MB (4096 MB allocatable), 8MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

INFO: All hashes found as potfile and/or empty entries! Use --show to display them.

Started: Wed Nov 12 20:49:30 2025
Stopped: Wed Nov 12 20:49:30 2025
❯ hashcat --show -m 1000 web_svc.hash3 --potfile-path=hashcat.potfile

❯ hashcat --show -m 1000 web_svc.hash3


# 为什么要用 `-m 5600`（而不是 `-m 1000` 或 `-m 0`）

- `-m 5600` = **Net-NTLMv2**（challenge/response blob）。hashcat 在这个模式下会把 `ntlmv2_response_blob` 与 `server_challenge` 一起，用候选明文生成 NT 哈希并做 HMAC-MD5 验证，从而判断明文是否正确。
    
- `-m 1000` = **NTLM（NT hash）**，适用于你已经有了目标账户的原始 NT 哈希（32 hex）。如果你只有一个 32 字节的 NT 哈希（不含 challenge/response），就用 1000。
    
- `-m 0` = **MD5**（另一个常见 32 hex 格式），与 NTLM 完全不同。
    

所以：**如果你有的是 NTLMv2 的 challenge/response，就用 5600；如果你只有单纯的 32-hex NT 哈希，就用 1000。**

# 4) 为什么 impacket 的那行可以直接给 hashcat（只要格式正确）

impacket 在代理/捕获 SMB 验证时会把 `username::domain:challenge:response` 这整行打印出来——这就是 hashcat 所期望的 Net-NTLMv2 输入格式。只要你把整行逐字保存到文件（每行一个 blob），hashcat 的 `-m 5600` 就能解析并尝试破解（需要字典/规则或掩码）。

- impacket 抓到的是**完整的 NTLMv2 challenge/response**，因此是长行、需要 `-m 5600`。
    
- 你之前从 Mimikatz 看到的尾 `#` 通常是复制粘贴/可视化字符导致的“脏数据”；清理掉不可见尾巴，选择正确的 `-m`，就能正确交给 hashcat 破解。


两者都能抓 NTLM/NTLMv2 凭证，但方法不同：**Responder** 用广播/中毒让大量主机自动认证（更主动、吵），**impacket smbserver** 则是被动提供 SMB 服务，只有被访问时才会抓到凭证（更定向、安静）。两者产出的凭证都能用 `hashcat -m 5600` 破解。


|**Mode**|**Hash Type**|**Signature / Format**|
|---|---|---|
|**13100**|**TGS-REP** (RC4)|`$krb5tgs$23$...`|
|**19600**|TGS-REP (AES-128)|`$krb5tgs$17$...`|
|**19700**|TGS-REP (AES-256)|`$krb5tgs$18$...`|
|**18200**|**AS-REP** (RC4)|`$krb5asrep$23$...`|
|**19800**|AS-REP (AES-128)|`$krb5asrep$17$...`|
|**19900**|AS-REP (AES-256)|`$krb5asrep$18$...`|


|**Mode**|**Hash Type**|**Signature / Format**|
|---|---|---|
|**1000**|**NTLM**|`32-char-hex-string` (e.g., dumped from SAM)|
|**3000**|LM|Old Windows hash, split into two 16-char halves|
|**5500**|NetNTLMv1|`username::hostname:LM:NTLM:challenge`|
|**5600**|**NetNTLMv2**|`username::domain:challenge:HMAC-MD5...`|
|**2100**|DCC2 (Domain Cached Credentials)|`$DCC2$10240#username#hash`|

|**Mode**|**Hash Type**|**Signature / Format**|
|---|---|---|
|**500**|md5crypt (MD5)|`$1$...`|
|**1800**|sha512crypt (SHA512)|`$6$...`|
|**3200**|**bcrypt** (Blowfish)|`$2a$`, `$2y$`, or `$2b$...`|



|**Mode**|**Hash Type**|**Signature / Format**|
|---|---|---|
|**0**|**MD5**|32 chars (e.g., `8743b52063cd84097a65d1633f5c74f5`)|
|**100**|SHA1|40 chars|
|**1400**|SHA2-256|64 chars|
|**1700**|SHA2-512|128 chars|


|**Mode**|**Hash Type**|**Context**|
|---|---|---|
|**13600**|WinZip|Zip files (formatted by `zip2john`)|
|**10500**|PDF (MD5)|Older PDF files|
|**25400**|PDF (1.7 Level 8)|Newer PDF files|
|**9600**|MS Office 2013|Word/Excel documents|


|**Mode**|**Hash Type**|**Context**|
|---|---|---|
|**22000**|**WPA-PBKDF2-PMKID+EAPOL**|The modern standard for WPA2/WPA3 (combines old 2500 & 16800)|

# View all examples with signatures
hashcat --example-hashes | less

# Grep for a specific name (e.g., "Roasting")
hashcat --help | grep -i "Kerberos"