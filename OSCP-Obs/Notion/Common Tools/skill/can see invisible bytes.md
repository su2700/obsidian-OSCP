
# 用可视化方式看控制字符（^M 表示 CR）
```

cat -v web_svc.hash2
```
# 十六进制查看前几字节
```

hexdump -C web_svc.hash2 | sed -n '1,2p'
```
# 或 xxd
```

xxd -g 1 web_svc.hash2 | sed -n '1,2p'

```


fix
# 方法 A：保留#前面的字段（最安全）
```
awk -F'#' '{print $1}' web_svc.hash2 > /tmp/web_svc.hash2 && mv /tmp/web_svc.hash2 web_svc.hash2
```

# 方法 B：删除所有 CR 并去掉尾随空白和可能的 '#'
```

tr -d '\r' < web_svc.hash2 | sed 's/[[:space:]]*#$//' > /tmp/web_svc.hash2 && mv /tmp/web_svc.hash2 web_svc.hash2
```


# 方法 C：仅删除所有尾随空白（保留中间的#，如果有的话）
```
sed -i 's/[[:space:]]\+$//' web_svc.hash2
```

```
 awk -F'#' '{print $1}' web_svc.hash3 > web_svc.hash3.tmp && mv web_svc.hash3.tmp web_svc.hash3
```
