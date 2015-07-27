# 获取网站数据库服务器当前时间：SPUtility.GetServerNow
    作者：杨柳@水杉网络

## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
SharePoint有着优秀的可扩展拓扑结构。对于上规模的应用，一个服务器场中往往有多台服务器。在开发过程中，如果需要使用到服务器时间，该以那一台服务器为准呢？SharePoint服务器端是以数据库为核心的，所以数据库服务器的时间理所当然可以作为唯一标准。

## 方法
位于Microsoft.SharePoint.Utilities.SPUtility类，具体描述如下：
``` c#
//获取网站的数据库服务器当前UTC时间
public static DateTime GetServerNow(SPWeb web)
{
    if (web == null)
    {
        throw new ArgumentNullException("web");
    }
    return web.ServerNow;
}

```


## 使用场景
在多服务器场环境下进行开发，需要使用到能描述整个系统统一时间，可以考虑使用此方法获取网站所在数据库服务器时间作为统一时间。


## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 输出结果：2015年7月27日 14:10:12
# 输出结果视当前环境而定
#####
$w = Get-SPWeb http://localhost
[Microsoft.SharePoint.Utilities.SPUtility]::GetServerNow($w)
```
