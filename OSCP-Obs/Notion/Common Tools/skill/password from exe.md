```
strings -a admintool.exe | egrep -i 'pass|pwd|password|Enter administrator'
```
how to get a password from windows file

```
strings -a admintool.exe > admintool.strings && grep -Ei 'Rijndael|AES|salt|key|password' admintool.strings
```

