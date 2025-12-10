3.1.3、cron
使用命令find /var/spool/cron/ -type f -ls 2>/dev/null查看定时任务，没有结果。使用命令find /var/spool/cron/ -type f -ls不隐藏报错，发现是没有查看权限。



使用命令find /etc/*cron* -type f -ls 2>/dev/null查看定时任务，发现还挺多。使用命令find /etc/*cron* -type f -ls 2>/dev/null -exec cat {} \; > /tmp/cron.txt和grep '\*' /tmp/cron.txt查看定时任务内容，发现root用户每5分钟会执行一次/usr/local/sbin/cron-logrotate.sh脚本。
————————————————