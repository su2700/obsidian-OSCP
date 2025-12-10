```
nmap -p- -Pn $target -v --min-rate 1000 --max-rtt-timeout 1000ms --max-retries 5 -oN nmap_ports.txt \
&& sleep 5 \
&& nmap -Pn $target -sV -sC -v -oN nmap_sC.txt \
&& sleep 5 \
&& nmap -T5 -Pn $target -v --script vuln -oN nmap_vuln.txt

```
