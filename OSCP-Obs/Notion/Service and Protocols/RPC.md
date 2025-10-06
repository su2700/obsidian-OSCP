[[135]] **/tcp** — **Microsoft DCE/RPC Endpoint Mapper** (the first place to check on Windows/AD hosts).

[[445]]**/tcp** — **SMB** (many MSRPC services ride over SMB / named pipes).

[[139]]**/tcp** — legacy NetBIOS/SMB (older systems).

[[111]] **111/tcp & 111/udp** — **ONC (Sun) RPC portmapper** (common on *nix services like NFS, rpc.statd, mountd).

[[rpcclient]]

### **RPC (Remote Procedure Call)**

- **Definition:** A protocol that allows a program on one computer to execute code on another computer as if it were local.
    
- **Generic term:** RPC can refer to the concept or various implementations across operating systems (Linux, Windows, etc.).
    
- **Ports:** Can run over different transport protocols (TCP, UDP). On Windows, RPC often uses **dynamic ports above 1024** plus some well-known ports like 135.