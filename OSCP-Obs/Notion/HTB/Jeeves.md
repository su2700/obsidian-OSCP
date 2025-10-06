```Bash
smbclient -L "//10.10.10.63/" -U "guest"%

smbclient -L "//10.10.10.63/" -N

rpcclient -N 10.10.10.63
```

-U "guest"%: The -U option is used to specify the username to authenticate as when connecting to the SMB server. In this case, "guest" is the username. The % symbol following guest is used to indicate an empty password. This means the command attempts to connect as the guest user with no password.

  

Yes, microsoft-ds is indeed associated with SMB (Server Message Block) protocol on port 445. The term microsoft-ds stands for Microsoft Directory Services and is used by Windows systems for file sharing, printer sharing, and various other network services that rely on SMB.

**Understanding Microsoft-DS and SMB**

**SMB (Server Message Block):**

• SMB is a network file sharing protocol that allows applications and users to read and write to files and request services from server programs in a computer network.

• It provides shared access to files, printers, and serial ports and is used for communication between networked computers.

**Microsoft-DS:**

• Microsoft-DS is essentially a synonym for SMB over TCP/IP on port 445.

• The microsoft-ds service name is used to denote that SMB traffic is being handled on port 445, which is a direct TCP/IP implementation without the older NetBIOS layer.

**Port Usage**

• **Port 445 (TCP)**: This is the port used for direct hosting of SMB over TCP/IP. Modern versions of Windows use port 445 for SMB communications.

• **Ports 137-139 (TCP/UDP)**: These were used for NetBIOS over TCP/IP, which was the older method for SMB communications. This method is less common in modern networks.

  

**Port 137**: Used by NetBIOS Name Service (NBNS)

• **UDP** (most common) and **TCP** (less common)

• **Port 138**: Used by NetBIOS Datagram Service (NBDS)

• **UDP**

• **Port 139**: Used by NetBIOS Session Service (NBSS)

• **TCP**

• **Port 445**: Used by SMB directly over TCP/IP

• **TCP**