```
find / -name local.txt
```

```
`find / -perm -u=s -type f 2>/dev/null`
```
 -suid-getcap提权  
/usr/sbin/getcap -r / 2>/dev/null

.Cron job提权，寻找[定时任务](https://so.csdn.net/so/search?q=%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1&spm=1001.2101.3001.7020)并修改进行提权  
cat /etc/crontab