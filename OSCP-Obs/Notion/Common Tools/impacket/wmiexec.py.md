```
 impacket-wmiexec -hashes :3c4495bbd678fac8c9d218be4f2bbc7b Administrator@192.168.185.141
```
```
impacket-wmiexec -hashes :3c4495bbd678fac8c9d218be4f2bbc7b Administrator@192.168.230.147
```

| 项目          | `wmiexec.py`                        | `Evil-WinRM`                                                       |
| ----------- | ----------------------------------- | ------------------------------------------------------------------ |
| 📌 工具类型     | Impacket 工具集，使用 WMI 协议远程执行命令        | Ruby 脚本，通过 WinRM（Windows Remote Management）协议连接远程 Windows          |
| 📦 协议基础     | SMB + WMI（DCOM）                     | WinRM（HTTP/SOAP，5985/5986端口）                                       |
| 🔑 支持的认证    | 用户名 + 密码 / 哈希（Pass-the-Hash）        | 用户名 + 密码 / 哈希 / 证书（.pfx） / Kerberos                                |
| 🎯 使用前提     | - 必须有远程管理员权限- 开放端口445（SMB）- WMI服务正常 | - 目标开启 WinRM 服务（默认企业域环境常开启）- 5985 或 5986 端口开放- 用户是管理员或有远程 WinRM 权限 |
| 📋 执行方式     | 远程执行命令，无需上传文件                       | 支持命令执行、上传/下载文件、执行 PowerShell 脚本                                    |
| 🧪 交互性      | 简单的伪交互 shell（回显非实时，有限）              | 完整交互 PowerShell shell，体验非常接近本地                                     |
| 📁 文件操作     | 不直接支持上传下载，但可以手动构造 `echo` 写文件        | 内置 `upload`, `download`, `script`, `execute` 等命令                   |
| 🕵️‍♂️ 静默程度 | 非常静默，不创建服务，日志少                      | 可能触发 PowerShell 日志（4104），但隐蔽性也不错                                   |
| 🔧 安装依赖     | Python3 + Impacket                  | Ruby + gem 安装依赖包                                                   |
| 🌐 适用网络     | 内网常见（默认445开放）                       | 较少默认开放，常用于域控/企业机环境                                                 |
| 💻 常见用途     | 横向移动、静默执行命令                         | 横向移动、完整 Shell、文件操作、权限持久化                                           |