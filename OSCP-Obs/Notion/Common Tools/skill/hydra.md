
-e nsr：这是扩展选项：

n：尝试“空密码”（no password）；

s：尝试用户名作为密码；

r：尝试反转的用户名作为密码。

```
```bash
hydra -l offsec -P /usr/share/wordlists/rockyou.txt -M ip.txt ssh
```
```