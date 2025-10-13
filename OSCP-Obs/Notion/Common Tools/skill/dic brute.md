在 **Kali** 中，公认“最大、最全”的目录/内容字典来自 **SecLists**（作者 Daniel Miessler）的集合，特别是其 `Discovery/Web-Content` 和 `Discovery/Web-Directories` 目录下的那些大列表（例如 `raft-large-directories.txt` / `raft-large-files.txt`、`common.txt`、以及合并/扩展后的“大”版本）。另外，Kali 也自带一些 DirBuster/DirB 的大字典（`/usr/share/dirbuster/wordlists`）可用。

---

### 常见的“大”字典位置（Kali 默认路径）

- `/usr/share/wordlists/seclists/Discovery/Web-Content/`  
    这里是最常用的集合，包含多种粒度（small/medium/large/raft 等）。
    
- `/usr/share/wordlists/dirbuster/`  
    DirBuster 的历史字典（有些也很大）。
    
- `/usr/share/wordlists/` 下的其它文件（视 Kali 版本而异）。