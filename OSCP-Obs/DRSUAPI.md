Directory Replication Service Remote Protocol API


- **D**: **Directory**. This refers to the Active Directory database that stores all objects like users, computers, and groups.
    
- **R**: **Replication**. This describes the primary function of the protocol: synchronizing data between different Domain Controllers.
    
- **S**: **Service**. It indicates that this is a standardized network service running on the Windows server.
    
- **U**: **User** (often associated with **Username** in your tool commands). In the context of the protocol name itself, it relates to the **User Interface** or **Update** mechanisms within the Directory Service Remote Protocol.



> 我们采用了什么方法导出 NTDS.DIT​​ 文件？

**DRSUAPI**

**解释：**

**NTDS.dit**文件是 Windows 域控制器上 Active Directory 数据库的关键组件。该文件包含敏感信息，例如用户密码哈希值，这些信息可能被用于未经授权访问域。

DRSUAPI （目录复制服务远程协议 API）是 Microsoft 远程过程调用 (RPC) 接口**，**允许在域控制器之间复制目录数据，例如 NTDS.dit 文件。

**Impacket 的** **secretdump.py**脚本还可以使用 DRSUAPI 执行**DCSync**攻击，从而冒充域控制器并请求复制 NTDS.dit 文件。