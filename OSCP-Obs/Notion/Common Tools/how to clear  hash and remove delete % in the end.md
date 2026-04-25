remove %

echo >> hash
cat hash

```
sed 's/%$//' hash.fixed > hash.clean

```

```
tr -d ' \n\t' < fela.hash > fela.fixed
```

含义是：
-d ' \n\t'：删除 空格 / 换行 / tab

输出到新文件 fela.fixed




```
cat 2.txt | tr -d '\n>' > fela.hash

```
含义是：

- `cat 2.txt`：读文件
    
- `tr -d '\n>'`：
    
    - 删除 **换行符 `\n`**
        
    - 删除 **`>` 字符**
        
- 输出到 `fela.hash`
    

👉 结果：  
**多行被合并成一行，`>>` 被去掉**