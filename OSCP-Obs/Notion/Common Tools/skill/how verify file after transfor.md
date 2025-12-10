# 传输完成后验证（务必做）

发送前（Windows）：

`certutil -hashfile .\myfile.txt SHA256`

接收后（Kali）：

`sha256sum received_myfile.txt`

两者一致表示没有被篡改或编码改变。