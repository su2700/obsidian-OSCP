**Checklist:**

**Active Directory:**

- [x] Scan all TCP ports.
    
- [ ] Check ldap, rpc, smb with anonymous access. Check for public share.
    
- [ ] Find usernames: If you get access, look for usernames with stuff like netexec --rid-brute, --users, and enumdomuser with rpcclient to get list of users in domain without needed initial credentials. Check usernames with kerbrute.
    
- [ ] Test for asreproasting after you have collected usernames. If successful, try cracking.
    
- [ ] Check for kerberoasting after you have a username and password.
    
- [ ] Try authenticating with every possible protocol with those set of credentials. winrm, rdp, mssql, smb, rpc, ldap etc.
    
- [ ] Enumerate shares for every user you get access to. Every new user means you should recheck their shares.
    
- [ ] If you get shell as a user, check for privesc, and dump all hashes and collect them in a file for possible bruteforcing and lateral movement.
    
- [ ] Run bloodhound and check for basic attacks like DCSync. Also checks for other paths bloodhound may find.
    
- [ ] Check for Certificate based attacks with certipy
    
- [ ] If it's an AD network, check for additional network adapters in case of pivoting being needed. If so, set up a pivot and relay all tools over that "jumpbox" so you can target the other machines in the other network.
    
- [ ] Remember to bruteforce different protocols with credentials you have. Remember to use NTLM authentication too.
    
- [ ] Check UDP ports.
    
- [ ] Rescan if you're stuck, verify tools are working properly and you're running them properly.
    

**Windows Privesc:**

- [ ] Run whoami /all, then PowerUp, then WinPEAS
    
- [ ] This will give you all info tbh. Save output and slowly go over it.
    
- [ ] If you get completely stuck, you can manually check for things too. If that doesn't help you likely need to relearn the privesc attacks.