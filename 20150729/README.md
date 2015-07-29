# PowerShell介绍 第六回 WMI介绍
    作者：小敏

或许很多人和我一样，不知道WMI是什么，为什么要是用WMI，如何才能找到自己想要的WMI，说白了就是我都不知道我的这个脚本需求需要用到那个WMI的类，而且我也不知道WMI的类都有哪些，好多好多问号啊，怎么办！

## 什么是WMI

>1. WMI的全称是Windows Management Instrumentation，即Windows管理工具。它是Windows操作系统中管理数据和操作的基础模块。我们可以通过WMI脚本或者应用程序去管理本地或者远程计算机上的资源。

>2. WMI有一组API。我们不管使用VBScript、PowerShell脚本还是利用C#的来访问WMI的类库，都是因为WMI向外暴露的一组API。这些API是在系统安装WMI模块的时候安装的，通过他们我们能够能拿到我们想要的类。

>3. WMI有一个存储库。尽管WMI的多数实例数据都不存储在WMI中，但是WMI确实有一个存储库，用来存放提供程序提供的类信息，或者称为类的蓝图或者Schema。
WMI有一个Service。WMI总是能够响应用户的访问，那是因为它有一个一直运行的Windows服务，名字叫Winmgmt。停止这个服务，所有对WMI的操作都将没有反应。

>>总体来说，WMI包括了系统许许多多的信息：
>>
  •  机器信息：制造商、型号、序列号等
>>
  •  BIOS信息
>>
  •  OS信息
>>
  •  CPU信息：种类、制造商、速度、版本
>>
  •  服务器内存总量
>>
  •  磁盘信息：容量、格式等
>>
  •  网络信息：MAC、IP等
>>
  •  其他


## 如何使用WMI

1. 如果你连Get-WmiObject都不知道，但是你又想使用powershell命令来调用WMI来完成脚本需要，那么第一步你可以get-command “*wmi*”
2. Get-WmiObject被你找到了，但是你不会用怎么办，get-help啊！在此我就不重复描述了。


## 如何查找WMI类

WMI的类是以命名空间和继承层次方式组织的，呈树形结构。命名空间的根是root，在它的下面还有十几个命名空间，最常用的是root\cimv2。命名空间的信息存储在静态类“__namespace”，要查询当前命名空间下的所有命名空间，可以查看__Namespace类的实例。

我也没有发现一个好用的WMI类查找手册，但是可以通过以下命令找出win32开头的类:

`$i=0 
$Type = "Win32" 
$WMI = Get-WmiObject -List | Where-Object {$_.name -Match $Type}
Foreach ($Class in $WMI) {$Class.name | out-file –filepath e:\win32.csv -append; $i++}`

>当找出了类之后，你还得想知道这些类有什么属性，对不对？此时可以使用Get-Member
Get-WmiObject -class Win32_NetworkAdapterConfiguration | Get-Member

>简单来说就是，如果一个成员是方法，那么，我们可以调用它。如果一个成员是属性，我们则可以查看它的值。


>还可以优化一下，给他们排个序：
>`Get-WmiObject -class Win32_NetworkAdapterConfiguration | Get-Member -MemberType property [a-z]*`
>
如果要查找root\CIMV2下的object可以使用以下命令
`Get-WmiObject -List -Namespace root\CIMV2`

>如果要找和disk相关的类，可以使用以下命令：
`Get-WmiObject -List | Where-Object { $_.name -match 'disk'}`


>查看服务信息
>`get-wmiobject -class win32_service -namespace "root\cimv2" | format-list *`
>
>查看BIOS信息
>
>`get-wmiobject -class win32_bios -namespace "root\cimv2"`

好了，最后补充一个工具，可以用来查看WIM类的，下载链接：

http://www.microsoft.com/en-us/download/details.aspx?id=24045
该工具的使用就不介绍了，有兴趣的童鞋可以网上找下资料。我们下次再见。
