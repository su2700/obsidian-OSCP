```
 sudo nmap -sT --min-rate 10000 $ip -p- -oA ports
```

``` 
awk '/\/tcp/ {gsub("/tcp","",$1); print $1}' ports.nmap | paste -sd ","
```

```
grep "/tcp" ports.nmap | cut -d'/' -f1 | paste -sd ","
```

```
tports=$(grep "/tcp" ports.nmap | cut -d'/' -f1 | paste -sd ",")
```