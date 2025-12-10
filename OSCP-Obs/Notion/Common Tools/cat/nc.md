Common flags

- -v verbose
- -n numeric (no DNS)
- -z scan only (no data transfer)
- -u UDP mode
- -l listen mode (server)
- -p local/source port
- -s source IP
- -w timeout seconds
- -q seconds quit after EOF (useful for one‑shot transfers)
- -k keep listening after client disconnect (ncat/some nc builds)
- -e / --exec or --sh-exec (execute program on connection — often disabled in OpenBSD nc; use ncat or socat if missing)