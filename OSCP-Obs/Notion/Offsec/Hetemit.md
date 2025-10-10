
it have a port 50000 http, should have a look

步骤 1：观察输出

服务器返回 {'code'}。这表明：

服务器期望输入中包含一个名为 code 的键。

它返回一个包含键“code”的字典（可能是 Python dict）。

值未显示，这通常意味着它会处理您在 code 键下发送的任何内容。

步骤 2：猜测输入法

Web 浏览器通常在您访问页面时发送 GET 请求。您看到的只是返回的键“code”。

这表明服务器期望从 POST 请求中获取数据，而不是简单的 URL 访问。

步骤 3：确定 POST 格式

在 Web API 中，数据可以作为表单数据（--data "key=value"）或 JSON（-H "Content-Type: application/json" --data '{"key":"value"}'）发送。

最简单的猜测是表单数据。由于密钥是 code，您可以尝试：

curl -X POST --data "code=2*2" http://192.168.56.117:50000/verify

这里的 2*2 是服务器可能执行的一个示例表达式（在 CTF 或 Python 沙盒服务器中很常见）。

步骤 4：为什么您可以推断这一点

服务器显示 {'code'} → 需要一个 code 参数。

简单的 GET 请求只返回密钥 → 需要 POST 请求来提供值。

在 Web API 中，测试小型 Python 表达式很常见 → 尝试使用 "2*2" 作为值。

✅ 总结：看到 {'code'} 表示：

该服务器类似于 Python（返回字典）。

它需要带有密钥 code 的 POST 数据。

您可以发送类似 2*2 的表达式作为值来查看结果。



check user have capability to write
```
 find /etc -type f -writable 2> /dev/null
```

find
```
/etc/systemd/system/pythonapp.service
```

system service, auto run after reboot
