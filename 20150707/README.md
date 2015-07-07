# 获取目录在SharePoint安装路径下的全路径：SPUtility.GetGenericSetupPath
    作者：杨柳@水杉网络

## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
针对SharePoint 2013。

一般说来，SharePoint安装的默认路径是在`C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions`下，在安装SharePoint是也可以设置此路径。在SharePoint开发中，有时我们需要使用到这个路径，比如做上传文件时处理，文件首先保存到服务器端本地路径，这个时候我们可以考虑使用此方法获取一个文件夹的服务器端本地路径用于存放上传的临时文件，然后服务器端进行读取处理。

## 方法
位于Microsoft.SharePoint.Utilities.SPUtility类，具体描述如下：
``` c#
//获取文件夹在SharePoint安装目录下的全路径
public static string GetGenericSetupPath(string strSubdir)
{
    if (!string.IsNullOrEmpty(strSubdir) && IsSetupPathVersioned(strSubdir))
    {
        return GetVersionedGenericSetupPath(strSubdir, 14);
    }
    return GetVersionedGenericSetupPath(strSubdir, 0);
}


```


## 使用场景
正如上面的介绍，某些业务需求下，客户端上传文件到服务器端，特别是一些大文件，内容处理有复杂逻辑，很耗时，如果在客户端请求过程中处理，会导致页面反应慢甚至超时，占用IIS资源。这时推荐下面的方式：

* 页面请求复制把文件上传到服务器的某个路径。
* 服务器端程序定时或者等待触发处理文件（比如Timerjob，SPService）。

这个时候可以考虑把上传文件的文件夹放到SharePoint的安装目录下，可以规避部分权限问题。由于安装目录可以在安装的时候设置，所以需要通过此方法获取运行时服务器上的安装目录。


## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 输出结果：C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\ShuiShan.S2.Demo
# 输出结果视当前SharePoint安装情况
#####
[Microsoft.SharePoint.Utilities.SPUtility]::GetGenericSetupPath("ShuiShan.S2.Demo")

```
