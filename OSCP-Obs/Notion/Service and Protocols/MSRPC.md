# MSRPC (Microsoft Remote Procedure Call)
[[135]]
[[593]]
**MSRPC (Microsoft Remote Procedure Call)** is the modified version of DCE/RPC. It forms the basis of network-level service interoperability. MSRPC is the protocol standard for Windows processes that allows a program running on one host to execute a program on another host.

### **2. MSRPC (Microsoft RPC)**

- **Definition:** Microsoft’s implementation of RPC.
    
- **Extra features:**
    
    - Integrates with Windows services
        
    - Uses DCOM (Distributed COM) for some operations
        
    - Uses dynamic ports + port 135 for RPC Endpoint Mapper
        
- **Ports:**
    
    - TCP 135 → RPC Endpoint Mapper (tells clients which dynamic port to use)
        
    - TCP 49152–65535 → dynamic ports for actual RPC services (range varies by Windows version)