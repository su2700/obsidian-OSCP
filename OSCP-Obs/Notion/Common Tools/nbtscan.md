**NBT** stands for **NetBIOS over TCP/IP**.
[[137]]
[[139]]
[[445]]
[[138]]

- **139/tcp** = NetBIOS Session Service → SMB over NetBIOS.
    
- **445/tcp** = SMB over TCP (no NetBIOS).
    
- Windows may support either or both; some older hosts only have 139 open, some environments block 445 and only allow 139, and some disable NetBIOS entirely.
    
- NetBIOS name service is UDP **137**, and NetBIOS datagram service is UDP **138** — different from 139.


- **UDP/137** → NetBIOS Name Service (NBNS)
    
    - Resolves NetBIOS names to IPs (similar to DNS for NetBIOS).
        
    - Queried by tools like `nbtscan` and `nmblookup`.
        
- **UDP/138** → NetBIOS Datagram Service
    
    - Used for broadcasts, browsing announcements, etc.
        
- **TCP/139** → NetBIOS Session Service
    
    - Used by **SMB over NetBIOS** (the “classic” Windows file sharing transport before direct SMB on 445).