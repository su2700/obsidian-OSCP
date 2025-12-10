# 把到本机 6666 的 TCP 请求转发到 10.10.191.142:5985

```
sudo iptables -t nat -A PREROUTING -p tcp --dport 6666 -j DNAT --to-destination 10.10.191.142:5985

```

# 采用 MASQUERADE 保证回复包正确回到客户端


```
sudo iptables -t nat -A POSTROUTING -p tcp -d 10.10.191.142 --dport 5985 -j MASQUERADE

```

验证与查看：

```
sudo iptables -t nat -L -n -v sudo ss -tlnp | grep 6666  
``` 
# 看本机监听情况（注意 iptables 不一定显示进程）

delete
```
sudo iptables -t nat -D PREROUTING -p tcp --dport 6666 -j DNAT --to-destination 10.10.191.142:5985
sudo iptables -t nat -D POSTROUTING -p tcp -d 10.10.191.142 --dport 5985 -j MASQUERADE
```

# 方法 3 — socat（快速、无需内核表，用户态代理）

适合临时或不想改防火墙/NAT 的场景：
```
sudo apt install -y socat   
sudo socat TCP-LISTEN:6666,fork TCP:10.10.191.142:5985
```
# 后台运行
```

sudo nohup socat TCP-LISTEN:6666,fork TCP:10.10.191.142:5985 > /var/log/socat.log 2>&1 &

```
