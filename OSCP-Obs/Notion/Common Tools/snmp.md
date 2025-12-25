`NET-SNMP-EXTEND-MIB::nsExtendObjects` 是 Net-SNMP（Linux/Unix 系统上最常用的 SNMP 服务端软件）中一个非常强大但也极易被配置错误的扩展功能。

在渗透测试中，这里往往是**寻找敏感信息（如密码、配置详情）的金矿**。

以下是关于它的详细技术解释，以及它为何会泄露“jack”的密码：

### 1. 它是做什么的？(原理)

标准的 SNMP MIB（管理信息库）通常只包含预定义的数据，比如 CPU 使用率、网络流量、硬盘剩余空间等。

但是，系统管理员有时需要监控一些标准 MIB 没有涵盖的特定数据（例如：某个特定服务的运行状态、特定的日志内容）。Net-SNMP 提供了一个扩展机制，允许管理员在配置文件（`snmpd.conf`）中定义**自定义命令或 Shell 脚本**。

- **机制：** 当 SNMP 代理（服务端）收到对 `NET-SNMP-EXTEND-MIB` 的查询请求时，它会执行管理员预设的脚本。
    
- **结果：** 脚本的**标准输出 (STDOUT)** 会被捕获，并通过 SNMP 协议返回给查询者。
    

### 2. 为什么密码会出现在这里？(场景还原)

在你的攻击场景中，管理员在服务器的 `/etc/snmp/snmpd.conf` 文件中可能添加了类似这样的配置：

Bash

```
# /etc/snmp/snmpd.conf 配置示例 (导致漏洞的配置)
extend check_creds /bin/cat /opt/scripts/credentials.txt
```

或者是一个脚本：

Bash

```
extend server_health /bin/bash /opt/check_status.sh
```

而这个 `check_status.sh` 脚本在执行过程中，可能为了连接数据库或其他服务，打印出了类似 "User: uname, Pass: passwd" 的调试信息。

当攻击者运行 `snmpwalk ... NET-SNMP-EXTEND-MIB::nsExtendObjects` 时，SNMP 服务端就老老实实地运行了这个命令，把结果（包含密码）全部吐了出来。

### 3. 攻击者看到的详细结构

当你针对 `nsExtendObjects` 进行枚举时，你实际上是在查看 Net-SNMP 扩展功能的整个输出树。这个树通常包含以下几个关键部分：

- **nsExtendCommand:** 显示了管理员配置的具体命令路径（例如 `/bin/cat`）。
    
- **nsExtendArgs:** 显示了命令的参数（例如 `/opt/credentials.txt`）。
    
- **nsExtendOutput1Line:** 显示命令输出的第一行。
    
- **nsExtendOutputFull:** 显示命令的**完整多行输出**（这是最重要的地方，密码通常藏在这里）。
    

**模拟输出结果：**

Plaintext

```
NET-SNMP-EXTEND-MIB::nsExtendCommand."test" = STRING: /bin/echo
NET-SNMP-EXTEND-MIB::nsExtendArgs."test" = STRING: "jack Not2Easy4Win8!"
NET-SNMP-EXTEND-MIB::nsExtendOutput1Line."test" = STRING: jack Not2Easy4Win8!
NET-SNMP-EXTEND-MIB::nsExtendOutputFull."test" = STRING: jack Not2Easy4Win8!
```

### 4. 总结

- **`NET-SNMP-EXTEND-MIB`** 是 Net-SNMP 允许执行自定义脚本的接口。
    
- **`nsExtendObjects`** 是包含这些脚本执行结果的对象表。
    
- **漏洞成因：** 管理员把包含敏感信息的脚本挂载到了 SNMP 扩展中，且没有限制 SNMP 的访问权限（使用了容易被猜到的 Community String）。
    

**这就是为什么在渗透测试的“信息收集”阶段，枚举 SNMP 的扩展 MIB 是必做动作之一。**

