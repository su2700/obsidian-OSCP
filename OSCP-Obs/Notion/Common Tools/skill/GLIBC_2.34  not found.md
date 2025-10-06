charles@pelican:/tmp$ /tmp/ld-linux-x86-64.so.2   --library-path /tmp   /tmp/pspy64
/tmp/pspy64: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.34' not found (required by /tmp/pspy64)
/tmp/pspy64: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.32' not found (required by /tmp/pspy64)



```
/tmp/ld-linux-x86-64.so.2   --library-path /tmp   /tmp/pspy64
```
1. Download a `libc6` `.deb` for x86_64 (choose a distro/release that ships glibc ≥ 2.32 / 2.34).
    
    - If you have `apt` and a repo that contains a newer libc, you can do:
        
        `apt download libc6`
        
        (That downloads whatever `libc6` is available in your apt metadata to `./libc6_*.deb`.)
        
    - Or fetch the appropriate `.deb` from your distro package site (packages.debian.org, packages.ubuntu.com, etc.). _(If you want, tell me your distro/version and I can suggest exact package names.)_
        
2. Extract the `.deb` into a local dir:
    

`mkdir ~/glibc-local dpkg-deb -x libc6_*.deb ~/glibc-local`

get two file from libc6



charles@pelican:/tmp$ bash l
ld-linux-x86-64.so.2             libc.so.6
