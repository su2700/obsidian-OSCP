### ✅ Option 1: Use `-q` (quit after EOF)

Most modern versions of `nc` (like GNU netcat or OpenBSD netcat) support the `-q` option:

- **Sender:**
    
    bash
    
```
  nc -q 0 192.168.1.100 1234 < file.zip
   ```
    
    - `-q 0` tells `nc` to quit immediately after reaching EOF on stdin (after sending the file).
        
- **Receiver:**
    
    bash
  ```
   nc -l -p 1234 > received.file
   ```
This way, once the file is fully sent, the sender closes the connection, and the receiver exits too.

### ✅ Option 2: Add `-N` (close on EOF)

On some `nc` versions (like OpenBSD’s), you can use:

bash

```
nc -N 192.168.1.100 1234 < file.zip
```

- `-N` makes `nc` close the connection after EOF on stdin.
    

### ✅ Option 3: Use `pv` for progress + auto-close

If you want progress monitoring and auto-close:

bash

```
pv file.zip | nc -q 0 192.168.1.100 1234
```

- `pv` shows transfer progress.
    
- `-q 0` ensures `nc` quits after sending.



Here are practical, copy‑pasteable ways to send files with netcat (nc), plus safer alternatives and troubleshooting. Pick the section that matches your environment (GNU/Linux, OpenBSD netcat, ncat, need encryption, directories, progress, etc.).

Basic unencrypted file transfer (most common)

- Receiver (listening):  
```
nc -l -p 1234 > received.file
```
    
- Sender (connect & send):  
```
nc 192.168.1.100 1234 < file.zip
```

Notes:

- Replace 192.168.1.100 and port 1234 with the receiver IP/port.
- On some netcat builds (OpenBSD/netcat) the listen syntax is: nc -l 1234 > received.file
- Use nc -N on the sender if your netcat supports it to close the connection cleanly after EOF: nc -N 192.168.1.100 1234 < file.zip

Show transfer progress

- If pv is installed: pv file.zip | nc -N 192.168.1.100 1234
- Or on receiver: nc -l -p 1234 | pv -s $(stat -c%s file.zip) > received.file

Transfer a directory (tar stream)

- Receiver: nc -l -p 1234 | tar xvf - -C /destination/path
- Sender: tar czf - /path/to/dir | nc -N 192.168.1.100 1234

Use checksums to verify integrity

- On sender (create checksum and send both or compute locally): sha256sum file.zip > file.zip.sha256 nc -N 192.168.1.100 1234 < file.zip scp file.zip.sha256 user@192.168.1.100:/path
- On receiver (after receive): sha256sum -c file.zip.sha256

Or compute on each side after transfer and compare: sha256sum file.zip # on sender sha256sum received.file # on receiver

Encrypted transfer (simple, using OpenSSL)

- Receiver: nc -l -p 1234 | openssl enc -d -aes-256-cbc -pass pass:YourPass > received.file
- Sender: openssl enc -aes-256-cbc -pass pass:YourPass < file.zip | nc -N 192.168.1.100 1234
- Better: use TLS-capable ncat (from nmap) or openssl s_server/s_client.

ncat (nmap) with SSL (if available)

- Receiver: ncat --ssl -l 1234 > received.file
- Sender: ncat --ssl 192.168.1.100 1234 < file.zip

OpenSSL s_server / s_client (TLS with certs)

- Receiver: openssl s_server -accept 1234 -cert cert.pem -key key.pem -quiet > received.file
- Sender: openssl s_client -connect 192.168.1.100:1234 -quiet < file.zip

Firewalls / permissions / options

- Make sure the listener port is allowed by firewall on receiver (ufw/iptables/firewalld).
- If the server should accept multiple transfers, use -k (if supported): nc -l -k -p 1234 > received.file (be careful, -k may append).
- Bind to an interface: nc -s 10.0.0.5 192.168.1.100 1234 < file.zip

Common pitfalls and notes

- Netcat does not provide reliability, resume, or integrity by itself — use checksums or rsync/ssh for reliable transfers and resumption.
- Some netcat variants behave differently: OpenBSD netcat, GNU netcat (traditional), busybox nc, and ncat differ in flags (-N, -q, -k). Use nc --help to confirm.
- Using plain nc sends data unencrypted — avoid over untrusted networks. Use ssh/rsync over ssh or TLS (ncat/openssl) instead for sensitive data.
- To avoid data corruption, ensure binary mode is respected (on Windows clients, use appropriate tools).

If you want a minimal secure alternative (recommended)

- Use scp (over SSH): scp file.zip user@192.168.1.100:/path/
- Or rsync over SSH (resume capable, efficient): rsync -av --progress file.zip user@192.168.1.100:/path/

Quick checklist for your environment

1. On receiver run appropriate listening command for your nc build (test with a small text file first).
2. Open the port in firewall if needed.
3. Run sender command.
4. Verify checksum on receiver.

If you tell me:

- which OS and nc build (output of which nc && nc --version or nc -h), and
- whether you need encryption or directory transfer and whether you want progress display,