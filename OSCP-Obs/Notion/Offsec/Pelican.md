https://talosintelligence.com/vulnerability_reports/TALOS-2019-0790

CVE-2019-5029
  

**Exhibitor for ZooKeeper**

this two name can try searchsploit

  

  

```SQL
find / -type f -perm -u=s -ls 2>/dev/null
- find / : Start searching from the root directory
- -type f : Only look for regular files (not directories, symlinks, etc.)
- -perm -u=s : Find files with the SUID bit set (alternative syntax)
- -ls : Display detailed information about each file found
- 2>/dev/null : Redirect error messages to /dev/null




find / -perm -4000 -user root -ls 2>/dev/null
- find / : Start searching from the root directory
- -perm -4000 : Find files with SUID permission bit set (4000 in octal)
- -user root : Only show files owned by the root user
- -ls : Display detailed information about each file found
- 2>/dev/null : Redirect error messages (like "permission denied") to /dev/null
```

  

```SQL
ps aux | grep password-store
ps aux - Lists all running processes with detailed information
- a shows processes from all users
- u provides detailed user-oriented format
- x includes processes not attached to a terminal

pgrep password-store
Filters the output to only show lines containing "password-store"
```

  

To find out which commands can be run with sudo without a password, you can use:

```Shell
sudo -l
```

This command will list all commands that the current user can run with sudo, including any that can be executed without a password (marked with NOPASSWD). In this case, it would show that `/usr/bin/gcore` can be run as root without requiring a password.

This is a crucial privilege escalation enumeration step and should be one of the first commands to run when assessing a system's security.

  

```SQL
/usr/bin/script -qc /bin/bash /dev/null


- /usr/bin/script : The script command that records terminal sessions
- -q : Quiet mode, don't display the start and done messages
- -c /bin/bash : Run bash shell as the command to record
- /dev/null : Discard the typescript file (don't save the recording to disk)

python3 -c 'import pty; pty.spawn("/bin/bash")'
python -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm

perl -e 'exec "/bin/bash -i";'

```

```SQL
bash-4.2$ ^Z
zsh: suspended  sudo nc -nlvp 33060


stty raw -echo;fg;reset;
```