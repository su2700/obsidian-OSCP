
如何利用 Linux 系统中的配置疏忽——将普通用户加入 `docker` 组——来获取系统的最高权限（Root）。

这是一种非常经典的**提权（Privilege Escalation）**手法。以下是详细的中文解析：

### 1. 核心漏洞：`docker` 组的危险性

> `We are r00t, not root. But more importantly, we're in the docker group.`

- **现状**：你当前登录的用户叫 `r00t`（注意这是个普通用户，UID 1001），**不是**真正的系统管理员 `root`（UID 0）。
    
- **关键点**：`id` 命令显示你属于 `docker` 组 (`groups=...116(docker)`)。
    
- **原理**：在 Linux 上，Docker 守护进程（Daemon）是直接以 `root` 权限运行的。如果你在 `docker` 组里，你就可以向 Docker 发送指令。这意味着你可以利用 Docker 这个“代理人”去执行任何只有 root 才能执行的操作。**只要进了 docker 组，本质上就等于拥有了 root 权限。**
    

### 2. 攻击命令详解

> `docker run -v /:/mnt --rm -it bash chroot /mnt sh`

这条命令实现了从“普通用户”到“宿主机 Root”的飞跃。我们可以把它拆解为三个步骤：

1. **挂载宿主机根目录 (`-v /:/mnt`)**：
    
    - `-v` 是挂载卷（Volume）的意思。
        
    - `/:/mnt` 的意思是：把**宿主机真实的根目录 `/`** 映射到 **容器内部的 `/mnt` 目录**。
        
    - **后果**：容器内部现在可以随意读写宿主机上的任何文件（包括密码文件、SSH 密钥等）。
        
2. **启动容器 (`docker run ... -it bash`)**：
    
    - 启动一个带有交互式终端（`-it`）的基础镜像（这里简写为 `bash`，通常实际操作中会用 `alpine` 或 `ubuntu` 镜像）。
        
3. **逃逸并切换环境 (`chroot /mnt sh`)**：
    
    - `chroot /mnt`：这是最致命的一击。它将当前进程的“根目录”切换到 `/mnt`。
        
    - 因为 `/mnt` 实际上就是宿主机的 `/`，所以当你执行完这一步，**你虽然身在容器进程中，但你的文件系统视角已经完全变成了宿主机的视角，而且你是以 Root 身份在运行 Docker 的。**
        

### 3. 获取战利品

> `cat /root/.ssh/authorized_keys/id_rsa`

- **目的**：现在你已经是宿主机的 Root 了，可以直接读取只有管理员能看的文件。
    
- **操作**：读取 Root 用户的 SSH 私钥（`id_rsa`）。拥有这个密钥后，你就可以直接通过 SSH 以 Root 身份远程登录这台机器，获得一个稳定的 Shell。 _(注：原文路径写得有点混淆，通常私钥在 `/root/.ssh/id_rsa`，而 `authorized_keys` 是放公钥的文件，但攻击意图非常明确：拿密钥。)_
    

### 总结

这是一个**“利用 Docker 组权限挂载宿主机文件系统”**的提权攻击。它告诉我们在配置 Linux 服务器时，千万不要随便把非信任用户加入 `docker` 组。