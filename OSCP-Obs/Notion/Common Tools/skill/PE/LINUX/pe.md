
```
find / -xdev \( -perm -u=s -o -perm -g=s \) -exec ls -l {} \; 2>/dev/null

```