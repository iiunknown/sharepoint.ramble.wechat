# PowerShell脚本系列之导出某个文件夹下一级文件夹的安全权限
    作者：小敏

一个多月没有更新powershell系列了，也不算是太忙吧，主要是思路有点乱，不知道该如何继续这一系列的文章。前面学习的都是一些很基础的知识，要么就是一条条的命令，算不上真正意义上的脚本。

我也是个初学者，也特别想学会powershell的脚本编写，那么就把我的学习过程和大家一起分享，也供powershell的小白参考参考。

##编写脚本

第一步，由于某文件夹的路径是可变的，那么我们需要先定义一个参数

`param
(
$filepath="d:\test",
$aclfile="D:\log\acl.csv"
)`

第二步，获取文件夹以及其子目录文件夹，并且筛选出文件夹，不要把文件也筛选了

通过(Get-ChildItem -Path $filepath).attributes来查看文件夹和文件的属性到底有什么不同，这样就可以筛选出我们想要的文件夹了.可以从结果看到文件夹的属性是directory，而文件的属性是archive，所以定义文件夹变量的句子可以写成

`$folder=Get-ChildItem -Path $filepath | where {$_.attributes -eq "directory"}`

第三步，接下来应该考虑获取权限了，核心命令是get-acl

get-acl用法是-path，而上面我们获取到的是foldername,可以通过($folder).fullname来转换，则句子如：$folder | foreach {Get-Acl -Path $_.fullname}
运行上面句子可以看到access的属性，这才是我们的最终目的，所以再调节一下句子

`$access=(Get-Acl -Path $folder.fullname).access | where {($_.IdentityReference -ne "NT AUTHORITY\SYSTEM") -and ($_.IdentityReference -ne "BUILTIN\Users")}`

第四步，筛选我们要的属性，access出来了很多属性，其中FileSystemRights和AccessControlType以及IdentityReference是我们想要的


`foreach ($permission in $access)
{
$IdentityReference=$permission.IdentityReference;
$FileSystemRighst=$permission.FileSystemRights;
$AccessControlType=$permission.AccessControlType;
"$IdentityReference,$FileSystemRighst,$AccessControlType" | Out-File -Append -FilePath $aclfile
}`

## 运行脚本

PS C:\Users\administrator> cd c:\1

PS C:\1> .\45678.ps1 -filepath "C:\share" -aclfile "c:\log\acl.csv"


本来是想截图的，但是markdown放图片很不友好，嫌麻烦就写成纯文字了。
但是以上脚本运行的结果是很不友好的，没有显示哪些权限是哪个文件夹的。以下附上完整的脚本：

```
param
(
$filepath="d:\test",
$aclfile="D:\log\acl.csv"
)

$folder=Get-ChildItem -Path $filepath | where {$_.attributes -eq "directory"}

foreach ($folderpath in $folder)
{
$access=(Get-Acl -Path $folder.fullname).access | where {($_.IdentityReference -ne "NT AUTHORITY\SYSTEM") -and ($_.IdentityReference -ne "BUILTIN\Users")}

"($folderpath).fullname" | Out-File -Append -FilePath $aclfile

foreach ($permission in $access)
{
$IdentityReference=$permission.IdentityReference;
$FileSystemRighst=$permission.FileSystemRights;
$AccessControlType=$permission.AccessControlType;
"$IdentityReference,$FileSystemRighst,$AccessControlType" | Out-File -Append -FilePath $aclfile
}
}
```
