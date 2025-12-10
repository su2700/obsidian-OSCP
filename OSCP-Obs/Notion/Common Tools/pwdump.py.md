/usr/share/creddump7/pwdump.py


- creddump7 这个项目很老了，主要针对 **WinXP / 2003 / 早期 Win7** 的 hive 结构
    
- 现在你拿到的是 **Win10/Server 2016+ 的 SYSTEM/SAM**，注册表布局有些差异，它的解析逻辑跟不上
    
- 它硬编码去找某些 key（`Select`、`ControlSet001` 等），结果结构不完全一样，就报错了
    

**不是你的 SAM/SYSTEM 坏了，是工具太老了。**

---

### 2. `samdump2` 报错：

`Error reading ControlSet: _RegOpenKey`

同样道理：

- 这是更老一代的工具，作者当年主要适配 NT/2k/XP/Vista
    
- 对新版本 Windows 的 hive 结构支持不完善
    
- 它试图按固定路径打开 `HKLM\SYSTEM\ControlSet00x`，失败就直接崩