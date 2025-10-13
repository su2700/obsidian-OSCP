openssl passwd -1 password123

`-1` 是 `openssl passwd` 命令的一个参数，表示使用 **MD5-based password algorithm** 来生成加密密码哈希。

具体说明：

- `openssl passwd -1 <password>`  
    使用MD5算法生成密码的哈希值，输出格式类似于 `$1$salt$hashed`，这是Apache等服务常用的MD5密码格式。

Trae

Used 1 context

I see that we're having connectivity issues with the SaltStack repository, and the system package installation approach isn't working. Let's try a different approach using Python virtual environment:

1. First, create and activate a new virtual environment:

```Plain
python -m venv salt-env
```

```Plain
source salt-env/bin/activate
```

1. Install the required packages using pip:

```Plain
pip install salt PyYAML
```

1. After the installation completes, run your script while the virtual environment is active:

```Plain
python CVE-2020-11652.py
```

This approach should work better because:

- It isolates the dependencies from your system Python
- It doesn't require system-wide package installation
- It can install packages directly from PyPI without needing the SaltStack repository
- It will automatically handle all the necessary dependencies  
    If you want to deactivate the virtual environment later, you can simply run:

```Plain
deactivate
```

1. **解释型语言**（interpreter）通常可以直接把源文件交给解释器运行，扩展名只是方便识别：  
    例：Python、Bash、Perl、Ruby、Node.js（JS）等。你可以 `python file.txt`、`bash file`，也可以加 shebang 并 `chmod +x` 后直接 `./file`。
    
2. **编译型语言**（compiled）通常必须先编译成二进制或中间格式，才能运行：  
    例：Go、Rust、C、C++。你通常执行 `go build` / `rustc` / `gcc` 生成可执行文件再运行，扩展名不影响生成的可执行文件（Linux 下没有 `.exe` 必须名，Windows 下生成的是 `.exe`）。
    
3. **有运行时/虚拟机的语言**（VM/bytecode）需要先转为字节码或打包然后用运行时运行：  
    例：Java（`javac` -> `.class` -> `java` 或 `jar` + `java -jar`）、C#（编译成 IL 用 .NET 运行）。这类语言直接把源码传给 `java` 通常不行（有 `jshell` 交互器除外）。
    
4. **shebang（#!）只对类 Unix 可执行文件有意义**，且只适用于能被某个解释器直接执行的脚本。二进制或需要编译的源码放 shebang 没意义（系统不会把它交给编译器自动编译）。