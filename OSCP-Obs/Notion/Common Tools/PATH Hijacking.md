When running netstat, a non-absolute path is used, this can be abused by generating
your own binary file called netstat and copying it to the /dev/shm directory. Since
when running netstat, a non-absolute path is used, the utility will initially be searched
from the PATH environment variable, since the /dev/shm directory is first in the list,
and it is from there that netstat will be launched. 

这段描述非常准确，它解释了 Linux 系统中一种非常经典的提权（Privilege Escalation）技术，被称为 **“环境变量劫持” (PATH Hijacking)** 或 **“PATH 变量操纵”**。

在渗透测试（特别是 CTF 和 Linux 提权）中，这通常发生在一个**具有 SUID 权限的程序**或**管理员脚本**中，调用了 `netstat` 命令但没有使用绝对路径（即使用了 `netstat` 而不是 `/bin/netstat`）。

以下是用中文对这一攻击思路、原理和具体操作步骤的详细解释：

### 1. 核心原理：PATH 的查找顺序

在 Linux 中，当你输入一个命令（如 `netstat`）时，系统并不知道这个可执行文件具体在哪。它会去查看一个名为 `PATH` 的环境变量。

- `PATH` 是一串由冒号分隔的目录列表，例如：`/usr/local/bin:/usr/bin:/bin:/usr/sbin`。
    
- **查找规则：** 系统会**从左到右**依次在这些目录中查找名为 `netstat` 的文件。一旦找到第一个匹配项，就会立刻执行它，并停止查找。
    

### 2. 漏洞点 (The Vulnerability)

漏洞产生的条件必须同时满足以下两点：

1. **相对路径调用：** 目标程序（假设是一个名为 `vuln_script` 的 SUID 程序）在代码中直接写了 `netstat`，而不是 `/bin/netstat`。
    
2. **环境继承：** 目标程序在运行时，继承了攻击者（当前用户）的环境变量设置，或者攻击者有权限修改目标程序运行时的 `PATH`。
    

### 3. 攻击步骤详解 (Attack Workflow)

假设你已经发现了一个 SUID 程序，它在运行时会调用 `netstat`。

#### 第一步：准备恶意 Payload

既然攻击者知道系统会先去 `/dev/shm` 找（假设我们将把它放到 PATH 的最前面），我们就去那里创建一个假的 `netstat`。

`/dev/shm` (Shared Memory) 是一个绝佳的攻击位置，因为通常所有用户都对它有写权限 (World Writable)，且通常允许执行文件。

Bash

```
cd /dev/shm

# 创建一个恶意脚本，内容是开启一个 shell
# 因为如果是 SUID 程序调用它，这个 shell 就会拥有 root 权限
echo '#!/bin/bash' > netstat
echo '/bin/bash -p' >> netstat

# 赋予执行权限
chmod +x netstat
```

#### 第二步：操纵 PATH 变量

你需要将 `/dev/shm` 添加到 `PATH` 变量的**最前面**。这样系统在找命令时，第一眼看到的就是你的假文件。

Bash

```
export PATH=/dev/shm:$PATH
```

此时，你可以检查一下：

Bash

```
echo $PATH
# 输出应该是类似： /dev/shm:/usr/local/bin:/usr/bin:...
```

#### 第三步：触发漏洞

现在运行那个有漏洞的程序（目标 SUID 程序）。

Bash

```
./vulnerable_program
```

**发生了什么？**

1. `vulnerable_program` 以 Root 权限启动。
    
2. 程序内部执行到 `system("netstat ...")` 这一行。
    
3. 系统查看 `PATH`，发现第一个路径是 `/dev/shm`。
    
4. 系统在 `/dev/shm` 下找到了 `netstat`（你的恶意脚本）。
    
5. 系统以 Root 权限执行了你的脚本。
    
6. **结果：** 你获得了一个 Root Shell。
    

### 4. 如何防御 (Mitigation)

为了防止这种攻击，系统管理员或开发者应该：

1. **使用绝对路径：** 在脚本或代码中，永远使用 `/bin/netstat` 而不是 `netstat`。
    
2. **重置环境变量：** 在 SUID 程序或脚本的开头，显式地重置 PATH 变量，例如 `export PATH=/bin:/usr/bin`，确保不使用用户自定义的路径。
    
3. **权限控制：** 尽量避免给 `/dev/shm` 或 `/tmp` 目录下的文件执行权限（通过挂载选项 `noexec`），但这通常很难完全避免。

