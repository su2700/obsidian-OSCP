```
python3 -m http.server 80
```


> 使用 Python，您可以从任何目录启动 HTTP 服务器；但是，Apache 有一个 webroot 目录，默认情况下通常为**/var/www/html**。

要启动 Apache Web 服务器，请将要向受害者提供的文件放在**/var/www/html**中，然后运行以下命令：**systemctl start apache2**

现在，您将拥有一个在端口 80 上运行的 Web 服务器；如果您向 Web 服务器添加新文件，则需要使用以下命令重新启动服务以使它们可见：**systemctl restart apache2**