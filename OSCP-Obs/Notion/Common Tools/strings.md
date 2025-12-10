```
strings -a admintool.exe > admintool.strings && grep -Ei 'Rijndael|AES|salt|key|password' admintool.strings
```