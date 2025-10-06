  

  

  

  

```SQL
awk -F':' '{OFS=":"; $1=$2=""; NF-=2; sub(/^::/, ""); print}' hash | sed 's/:$//' > hashed.txt
```

```SQL
Get-Content hash | ForEach-Object {
    $parts = $_ -split ':'
    $result = $parts[2..($parts.Length - 3)] -join ':'
    $result = $result.TrimEnd(':')
    Write-Output $result
}
```

```SQL
sed 's/^[^:]*:[^:]*:\([^:]*:[^:]*\).*$/\1/' hash | sed 's/::*$//' > hashed3.txt
```