
https://github.com/urbanadventurer/username-anarchy


```
Set-Location ".\THM stuff\Services"  
#$Names = @("James Bold", "Rose Bud")  
$Names = Get-Content .\Potential_Usernames.txt  
$FQDN = "@services.local"  
"administrator" + "$FQDN" | Out-File .\Brute.txt -Append  
"guest" + "$FQDN" | Out-File .\Brute.txt -Append  
ForEach($Name in $Names)  
{  
  
$FirstName = $Name.Split('')[0]  
$LastName = $Name.Split('')[1]  
$FirstInitial = $FirstName.Substring(0,1)  
$LastInitial = $LastName.Substring(0,1)  
$MangledLast = $LastName.Substring(0,2)  
$MangledLast2 = $LastName.Substring(0,1)  
  
"$FirstName.$LastName" + "$FQDN" | Out-File .\Brute.txt -Append  
"$FirstName$LastName" + "$FQDN" | Out-File .\Brute.txt -Append  
"$FirstName-$LastName" + "$FQDN" | Out-File .\Brute.txt -Append  
"$FirstInitial$LastName" + "$FQDN" | Out-File .\Brute.txt -Append  
"$FirstInitial-$LastName" + "$FQDN" | Out-File .\Brute.txt -Append  
"$FirstInitial.$LastName" + "$FQDN" | Out-File .\Brute.txt -Append  
"$FirstName$MangledLast" + "$FQDN" | Out-File .\Brute.txt -Append  
"$FirstName$MangledLast2" + "$FQDN" | Out-File .\Brute.txt -Append  
}  
  
$Results = (Get-Content .\Brute.txt).Length  
Write-Host "Mishka generated $Results usernames."  
Write-Host "Copy/paste the contents of Brute.txt to /home/kali/Downloads/Wordlists/Brute and kerbrute."
```

这段脚本的作用是：  
👉 **根据一堆姓名，自动生成很多“可能存在的用户名”，并保存到 Brute.txt 文件里。**

常用于：

- 测试账号命名规则
    
- 构造用户名列表
    

---

## 📁 第一步：切换工作目录

`Set-Location ".\THM stuff\Services"`

**在干嘛？**

- 把当前工作路径切换到这个文件夹
    

**为什么重要？**

- 后面的文件（txt）都在这里读和写
    
- 不切换目录可能找不到文件
    

---

## 📄 第二步：读取姓名列表

`$Names = Get-Content .\Potential_Usernames.txt`

**在干嘛？**

- 从 `Potential_Usernames.txt` 读取姓名
    
- 每一行是一个名字，比如：
    
    `James Bold Rose Bud`
    

**结果是什么？**

- `$Names` 变成一个名字数组
    

---

## 🌐 第三步：定义域名

`$FQDN = "@services.local"`

**在干嘛？**

- 定义邮箱/域名后缀
    
- 之后所有用户名都会加上这个
    

例子：

`james.bold@services.local`

---

## 👤 第四步：手动加入常见账号

`"administrator" + "$FQDN" | Out-File .\Brute.txt -Append "guest" + "$FQDN" | Out-File .\Brute.txt -Append`

**在干嘛？**

- 直接把常见账号写进文件
    
- `-Append` 表示：不覆盖，继续加
    

**为什么？**

- 管理员、访客账号很常见
    
- 优先测试价值高
    

---

## 🔁 第五步：遍历每一个姓名

`ForEach($Name in $Names)`

**意思是：**

- 对每一个名字，重复下面所有操作
    

---

## ✂️ 第六步：拆分姓名

`$FirstName = $Name.Split('')[0] $LastName = $Name.Split('')[1]`

**在干嘛？**

- 把 `James Bold` 拆成：
    
    - FirstName = James
        
    - LastName = Bold
        

---

## 🔤 第七步：取首字母和缩写

`$FirstInitial = $FirstName.Substring(0,1) $LastInitial = $LastName.Substring(0,1)`

**结果：**

- J
    
- B
    

---

## ✏️ 第八步：生成“变形”的姓

`$MangledLast = $LastName.Substring(0,2) $MangledLast2 = $LastName.Substring(0,1)`

如果姓是 `Bold`：

- Bo
    
- B
    

**用途？**

- 有些系统用户名会被缩短
    

---

## 🧩 第九步：生成各种用户名格式

`"$FirstName.$LastName" + "$FQDN" "$FirstName$LastName" + "$FQDN" "$FirstName-$LastName" + "$FQDN" "$FirstInitial$LastName" + "$FQDN" ...`

**会生成什么？**

`james.bold@services.local jamesbold@services.local james-bold@services.local jbold@services.local j-bold@services.local j.bold@services.local jamesbo@services.local jamesb@services.local`

**每一行都会写进 Brute.txt**

---

## 📊 第十步：统计生成了多少用户名

`$Results = (Get-Content .\Brute.txt).Length`

**在干嘛？**

- 统计 Brute.txt 里一共有多少行
    

---

## 🖥️ 第十一步：输出提示信息

`Write-Host "Mishka generated $Results usernames."`

**作用：**

- 在终端显示结果
    
- 告诉你生成了多少用户名
    

---

## 📌 最后一行说明

`Write-Host "Copy/paste the contents of Brute.txt to /home/kali/Downloads/Wordlists/Brute and kerbrute."`

**意思是：**

- 提示你下一步怎么用这个文件
    
- 把生成的用户名复制到其他工具里使用
- 