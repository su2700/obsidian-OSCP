  

8443/tcp open ssl/https-alt AppManager

80: http

443: https

8443: it should be https, plus it have ssl/https-alt

  

use cmd change admin ps

```SQL
cmd
net user Administrator newpassword

pwsh
$username = "Administrator"  # Replace with the actual username if different
$newPassword = Read-Host -AsSecureString "Enter the new password"
Set-LocalUser -Name $username -Password $newPassword
```