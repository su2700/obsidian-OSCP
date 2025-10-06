
[[ike-scan]]  /usr/share/seclists/Miscellaneous/ike-groupid.txt for ike
[[500]]
```
while read line; do
(echo "Found ID: $line" && sudo ike-scan -M -A -n $line $ip) | grep -B14 "1 returned handshake" | grep "Found ID:"
;                                                                                                                 done < /usr/share/seclists/Miscellaneous/ike-groupid.txt
```
## 
## 这条命令的预期行为概述

- 对字典里的每个单词（ID）：
    
    1. 输出 `Found ID: <word>`（作为标记）；
        
    2. 对目标发起 `ike-scan` 探测（带入该 ID）；
        
    3. 如果 `ike-scan` 输出包含 `1 returned handshake`（或类似成功标志），则最终会打印 `Found ID: <word>` —— 表示该 ID 触发了可识别的握手响应（枚举到的“命中”）。
顶层：`while read line; do ... done < wordlist`

- `while read line; do ... done < /path/to/file`  
    逐行读取文件，把每一行存入变量 `line`，循环体对每个 `line` 运行一次。
    子 shell/分组：( ... )

(echo "Found ID: $line" && sudo ike-scan -M -A -n $line $ip)
把 echo 和 ike-scan 在一个子 shell 中顺序执行，把它们的 stdout 合并后再传到后面的 grep。&& 表示前一个命令成功后再运行下一个（echo 基本永远成功），所以相当于按顺序输出两部分内容。

用子 shell 的目的：把 Found ID: <id> 这行与随后的 ike-scan 输出放在一起，方便后面通过上下文筛选出哪个 ID 导致了“成功”响应。

echo "Found ID: $line"

先输出一行标记，例如 Found ID: EZ，用于在后面筛选成功结果时定位是哪一个 ID 导致了响应。
sudo ike-scan -M -A -n $line $ip

sudo：以 root 权限运行（ike-scan 需要 raw sockets）。

ike-scan：探测 IKE/IPsec（默认 UDP/500）用的工具。

-M：Main Mode 探测（主模式）。

-A：Aggressive Mode 探测（侵略模式）。

问题：-M 与 -A 是不同的模式，通常不应同时使用。想要发现泄露 identity/group 的应使用 -A（Aggressive）。

-n $line：把 $line 作为 identity/组名传递（不同 ike-scan 版本参数略有差异，推荐用 --id="$line" 更清楚）。

$ip：目标 IP（变量名必须事先定义，例如 ip=10.10.11.87）。

推荐写法：sudo ike-scan -A --id="$line" "$TARGET"