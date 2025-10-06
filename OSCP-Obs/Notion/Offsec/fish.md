sudo nmap -sC -sV -A --open 192.168.236.168  
Starting Nmap 7.95 ( [https://nmap.org](https://nmap.org/) ) at 2025-03-31 19:27 CEST  
Stats: 0:00:25 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan  
Service scan Timing: About 75.00% done; ETC: 19:28 (0:00:08 remaining)  
Stats: 0:01:02 elapsed; 0 hosts completed (1 up), 1 undergoing Traceroute  
Traceroute Timing: About 32.26% done; ETC: 19:28 (0:00:00 remaining)  
Nmap scan report for 192.168.236.168  
Host is up (0.038s latency).  
Not shown: 992 closed tcp ports (reset)  
PORT STATE SERVICE VERSION  
[[135]]135/tcp open msrpc Microsoft Windows RPC  
[[139]]139/tcp open netbios-ssn Microsoft Windows netbios-ssn  
[[445]]445/tcp open microsoft-ds?  
[[3389]]3389/tcp open ms-wbt-server Microsoft Terminal Services  
|_ssl-date: 2021-10-30T02:44:20+00:00; -3y152d14h44m38s from scanner time.  
| ssl-cert: Subject: commonName=Fishyyy  
| Not valid before: 2021-10-29T01:54:38  
|_Not valid after: 2022-04-30T01:54:38  
[[4848]]4848/tcp open http Sun GlassFish Open Source Edition 4.1  
|_http-title: Login  
|_http-server-header: GlassFish Server Open Source Edition 4.1  
7676/tcp open java-message-service Java Message Service 301  
8080/tcp open http Sun GlassFish Open Source Edition 4.1  
|_http-title: Data Web_  
_| http-methods:_  
_|_ Potentially risky methods: PUT DELETE TRACE  
|_http-server-header: GlassFish Server Open Source Edition 4.1  
8181/tcp open ssl/http Sun GlassFish Open Source Edition 4.1  
| ssl-cert: Subject: commonName=localhost/organizationName=Oracle Corporation/stateOrProvinceName=California/countryName=US  
| Not valid before: 2014-08-21T13:30:10  
|_Not valid after: 2024-08-18T13:30:10  
|_http-title: Data Web  
|_http-server-header: GlassFish Server Open Source Edition 4.1_  
_| http-methods:_  
_|_ Potentially risky methods: PUT DELETE TRACE  
|_ssl-date: TLS randomness does not represent time  
No exact OS matches for host (If you know what OS is running on it, see [https://nmap.org/submit/](https://nmap.org/submit/) ).  
TCP/IP fingerprint:  
OS:SCAN(V=7.95%E=4%D=3/31%OT=135%CT=1%CU=39181%PV=Y%DS=4%DC=T%G=Y%TM=67EAD0  
OS:DA%P=x86_64-apple-darwin22.6.0)SEQ(SP=102%GCD=1%ISR=10B%TI=I%CI=I%TS=U)S  
OS:EQ(SP=102%GCD=2%ISR=10E%TI=I%CI=I%TS=U)SEQ(SP=104%GCD=1%ISR=10D%TI=I%CI=  
OS:I%TS=U)SEQ(SP=106%GCD=1%ISR=10B%TI=I%CI=I%TS=U)SEQ(SP=FF%GCD=1%ISR=10B%T  
OS:I=I%CI=I%TS=U)OPS(O1=M578NW8NNS%O2=M578NW8NNS%O3=M578NW8%O4=M578NW8NNS%O  
OS:5=M578NW8NNS%O6=M578NNS)WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6=F  
OS:F70)ECN(R=Y%DF=Y%T=80%W=FFFF%O=M578NW8NNS%CC=N%Q=)T1(R=Y%DF=Y%T=80%S=O%A  
OS:=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%  
OS:Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0%S=  
OS:A%A=O%F=R%O=%RD=0%Q=)T7(R=N)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%R  
OS:IPCK=G%RUCK=G%RUD=G)IE(R=N)

Network Distance: 4 hops  
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:  
|_clock-skew: mean: -1248d14h44m37s, deviation: 0s, median: -1248d14h44m38s_  
_| smb2-time:_  
_| date: 2021-10-30T02:44:13_  
_|_ start_date: N/A  
| smb2-security-mode:  
| 3:1:1:  
|_ Message signing enabled but not required

TRACEROUTE (using port 443/tcp)  
HOP RTT ADDRESS  
1 36.44 ms 192.168.45.1  
2 36.42 ms 192.168.45.254  
3 36.52 ms 192.168.251.1  
4 36.55 ms 192.168.236.168

OS and Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .  
Nmap done: 1 IP address (1 host up) scanned in 72.71 seconds  
➜ fish