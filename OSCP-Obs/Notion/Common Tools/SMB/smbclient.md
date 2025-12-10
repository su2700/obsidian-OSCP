 smbclient.py vulnnet-rst.local/t-skid:'tj072889*'@10.65.170.13


then use  shares
to list all shares
# shares
ADMIN$
C$
IPC$
NETLOGON
SYSVOL
VulnNet-Business-Anonymous
VulnNet-Enterprise-Anonymous
# use ADMIN$
[-] SMB SessionError: code: 0xc0000022 - STATUS_ACCESS_DENIED - {Access Denied} A process has requested access to an object but has not been granted those access rights.
# use C$
[-] SMB SessionError: code: 0xc0000022 - STATUS_ACCESS_DENIED - {Access Denied} A process has requested access to an object but has not been granted those access rights.
# use NETLOGON
# ls
drw-rw-rw-          0  Wed Mar 17 00:15:49 2021 .
drw-rw-rw-          0  Wed Mar 17 00:15:49 2021 ..
-rw-rw-rw-       2821  Wed Mar 17 00:18:14 2021 ResetPassword.vbs
#