# PowerShell脚本系列之修改某文件夹下文件名
    作者：小敏

哈哈，今天的推文依旧是小敏阿姨来推送。此时应该有掌声，也欢迎小伙伴们打赏。

今天学习写一个比较好玩的脚本，场景描述大致如下：

1. 某文件下下面有文件，命名不规范，现在我们要把他们的名字改成只有6个字，不够的就在前面添加A。超过6位的，跳过不修改。下次再丰富脚本，如何实现超过6的就取前面6位。

##1.编写脚本

第一步，由于某文件夹的路径是可变的，那么我们需要先定义一个参数。

`param
(
$logpath="D:\test",
$log="d:\log\log.txt"
)`

第二步，获取文件夹以及其子目录文件夹，并且筛选出文件，例如只修改txt的

`$filegroup=Get-childItem -path $logpath *.txt -recurse;`

第三步，获取文件名长度

`$filelenght=$file.Name.Length;`

第四步，判断文件名是否满足长度要求

`for($i=$filelenght;$i -lt 10;$i++)
{
$filename=$filename+"A"
}`

第五步，定义新文件名规则

`$newfilename=$filename+$file.Name;`

第六步，更新文件名

`Rename-Item $file.Name -NewName $newfilename;`

第七步，将执行日志写入log.txt

`$time=(get-date).ToString("yyyy-MM-dd HH:mm:ss");`
`$logwrite="您在"+$time+"将"+$file.name+"修改为"+$newfilename;`
`$logwrite | Out-File -Append -FilePath $log;`

## 2.运行脚本

PS C:\Users\administrator> cd c:\1

PS C:\1> .\45678.ps1 -logpath "D:\test" -alog "d:\log\log.txt"

##3.完整脚本

`param
(
$logpath="D:\test",
$log="d:\log\log.txt"
)`

`$filegroup=Get-childItem -path $logpath *.txt -recurse;`

`foreach ($file in $filegroup){`

`$filelenght=$file.Name.Length;`

`$filename=''`

`for($i=$filelenght;$i -lt 10;$i++){`

`$filename=$filename+"A"}`

`$newfilename=$filename+$file.Name;`

`Rename-Item $file.Name -NewName $newfilename;`

`$time=(get-date).ToString("yyyy-MM-dd HH:mm:ss");`

`$logwrite="您在"+$time+"将"+$file.name+"修改为"+$newfilename;`

`$logwrite | Out-File -Append -FilePath $log;}`