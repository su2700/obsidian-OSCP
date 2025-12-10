```
.\naabu.exe -p 1-65535 -host 10.10.104.142
```

- **Naabu** 是 ProjectDiscovery 开发的一个快速端口扫描工具（用 Go 写的，目标是高效、可靠地枚举目标主机开放端口）。[docs.projectdiscovery.io+1](https://docs.projectdiscovery.io/opensource/naabu/install?utm_source=chatgpt.com)
    
- `-p 1-65535`  
    这是告诉 naabu 扫描的端口范围 —— 从端口 **1 到 65535**（也就是 TCP 的所有常见端口区间）。换句话说，naabu 会对目标的每个端口发起探测以识别哪些端口是开放的。