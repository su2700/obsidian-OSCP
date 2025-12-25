```
(Get-Acl ‘dc=spookysec,dc=local’).access | Where {$_.IdentityReference -like “*backup*”}
```
backup is account name 
