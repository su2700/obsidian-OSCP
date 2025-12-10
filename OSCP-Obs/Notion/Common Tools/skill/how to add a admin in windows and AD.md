- Create domain user and add to Domain Admins: net user titan titan123 /add /domain net group "Domain Admins" titan /add /domain
- Alternative using dsadd (replace DC parts with your domain): dsadd user "CN=titan,CN=Users,DC=example,DC=com" -pwd titan123 dsadd group "CN=Domain Admins,CN=Users,DC=example,DC=com" -member "CN=titan,CN=Users,DC=example,DC=com"
- Verify: net user titan /domain net group "Domain Admins" /domain

4. Domain account — PowerShell (recommended; requires ActiveDirectory module / RSAT)

- Example (replace example.com / DC=... as needed): Import-Module ActiveDirectory $pw = ConvertTo-SecureString "titan123" -AsPlainText -Force New-ADUser -Name "titan" -SamAccountName "titan" -UserPrincipalName "titan@example.com" -AccountPassword $pw -Enabled $true -Path "CN=Users,DC=example,DC=com" -ChangePasswordAtLogon $false -PasswordNeverExpires $true Add-ADGroupMember -Identity "Domain Admins" -Members "titan"
- Verify: Get-ADUser -Identity titan Get-ADGroupMember -Identity "Domain Admins" | Where-Object {$_.SamAccountName -eq "titan"}

5. Cleanup (remove created accounts)

- Local CMD: net user titan /delete
- Local PowerShell: Remove-LocalUser -Name "titan"
- Domain CMD: net user titan /delete /domain
- Domain PowerShell: Remove-ADUser -Identity "titan" -Confirm:$false

Notes / troubleshooting

- If password creation fails, check domain password policy (Group Policy). Use a compliant password or consult the domain admin.
- If Add-ADGroupMember fails, ensure replication has completed and that the account exists in the same domain context you target (same DN / UPN).
- Actions will be audited; perform only with authorization and document the test scope/time.