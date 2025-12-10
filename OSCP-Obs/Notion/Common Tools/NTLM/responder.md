[[Notion/Common Tools/impacket/ntlmrelayx.py]]

[[smbserver.py]]

```
sudo responder -I tun0
```

两者都能抓 NTLM/NTLMv2 凭证，但方法不同：**Responder** 用广播/中毒让大量主机自动认证（更主动、吵），**impacket smbserver** 则是被动提供 SMB 服务，只有被访问时才会抓到凭证（更定向、安静）。两者产出的凭证都能用 `hashcat -m 5600` 破解。