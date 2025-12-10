
虽然 Kali 能识别 UTF-16LE，但：

1. **几乎所有 Linux/UNIX 工具链默认只处理 UTF-8。**  
    `grep`、`awk`、`sed`、`head`、`cat`、`less` 等不会主动把 UTF-16 解码；  
    结果要么显示乱码，要么什么都搜不到。
    
2. **UTF-16 文件在命令行输出中容易混乱。**  
    每个字符占 2 字节，BOM 和换行符序列（`\r\0\n\0`）让很多工具误判行尾，  
    例如 `wc -l` 数不准行数，`diff` 看起来全文件不同。
    
3. **UTF-8 是现代跨平台通用格式。**
    
    - Linux 默认 locale 是 UTF-8；
        
    - Git、Python 源文件、JSON、YAML、shell 脚本都假定 UTF-8；
        
    - 浏览器、API 接口也普遍使用 UTF-8。
        

> ✅ 简而言之：  
> “能识别 UTF-16” ≠ “所有工具都能正常处理”。  
> 为了避免各种小坑，**落地到 Kali 或 Linux 上建议转成 UTF-8**。

use pwsh converter utf16  to 8

```
if (Test-Path '.\myfile.txt') { $c=[System.IO.File]::ReadAllText((Get-Item '.\myfile.txt').FullName,[System.Text.Encoding]::Unicode); [System.IO.File]::WriteAllText((Join-Path (Split-Path (Get-Item '.\myfile.txt').FullName) 'myfile.utf8.txt'),$c,[System.Text.Encoding]::UTF8); Write-Output 'Converted to myfile.utf8.txt' } else { Write-Error 'myfile.txt not found' }
```

in Kali from 16 to 8

```
iconv -f UTF-16LE -t UTF-8 myfile.txt > myfile.utf8.txt
```

check which utf in kali
```
file -i myfile.txt
```

