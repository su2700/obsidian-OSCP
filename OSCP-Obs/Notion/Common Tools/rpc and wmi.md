
**RPC（Remote Procedure Call，远程过程调用）**

**DCOM（Distributed Component Object Model）** 是 Windows 的一种分布式组件技术，用来让程序在 **远程主机** 上执行对象、调用方法或访问资源。

# WMI（Windows Management Instrumentation）是什么？

**WMI = Windows 系统的管理接口框架**  
它允许远程或本地查询系统信息、管理资源、执行操作。

简单理解：

> **WMI 是 Windows 的“系统 API”，用来远程获取信息或执行管理任务。**

DCOM 是 WMI 的基础通信机制，它通过 RPC 进行远程调用。  
所以当你看到 “Allow anonymous login” 时，问题是在 RPC/DCOM 层，而不是 WMI 本身。


WMI 本质上就是构建在 DCOM 之上的。

结构如下：

`WMI   ↓ DCOM   ↓ MS-RPC（基于 SMB 445）`

因此：

- 你使用 **WMI** 远程执行命令时，底层实际上调用的是 DCOM
    
- DCOM 再通过 **RPC** 和目标主机通信
    

这就是为什么：

👉 当 NXC（NetExec）使用 `wmi` 模块时，需要建立一个 **DCOM/RPC 会话**  
👉 如果 RPC 允许匿名登录，NXC 就会报告 “anonymous login allowed”

但这并不意味着 WMI 可以匿名执行命令。  
它只是表示 **RPC 层** 允许匿名连接。