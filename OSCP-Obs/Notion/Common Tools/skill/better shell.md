```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

```
python -c 'import pty; pty.spawn("/bin/bash")'
```

```
/usr/bin/script -q -c bash /dev/null
```

```
socat STDIO EXEC:/bin/bash,pty,stderr,setsid,sigint,sane
```

```
/bin/bash -i
```
