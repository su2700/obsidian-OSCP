[[smbserver.py]]
# SMB (Server Message Block)

**`Default Port: 139, 445`**

**SMB (Server Message Block)**, also known as CIFS (Common Internet File System), is a network protocol that allows for file sharing, network browsing, printing services, and inter-process communication over a network.

The SMB protocol provides you with the ability to access resources from a server.


- **SMB itself** is the file sharing protocol.
    
- But **early SMB (Windows for Workgroups, NT, 95/98, etc.)** didn’t run directly over TCP/IP.
    
- Instead, it was encapsulated inside **NetBIOS**. That’s why those NetBIOS-related ports come into play.

- **UDP [[137]]– NetBIOS Name Service**
    
    - Used for name resolution (like DNS, but for NetBIOS names).
        
    - Clients ask "who is SERVER1?" and get the IP back.
        
- **UDP [[138]]– NetBIOS Datagram Service**
    
    - Used for broadcasts and datagram messages (e.g., announcements).
        
- **TCP[[139]] – NetBIOS Session Service**
    
    - This is where SMB traffic was actually carried when running over NetBIOS.
        
    - In other words: SMB = payload, TCP 139 = transport channel.


build smb server
[[smbserver.py]]
