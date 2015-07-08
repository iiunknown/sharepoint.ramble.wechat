# SPUtility.GetDisplayUserName

## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
SharePoint 2013默认是基于声明的身份验证，用户在验明正身之后传统的SPUser.LoginName被返回的声明标识替换，比如：

    i:0#.w|sp\spadmin

其中登录用户的显示名也包含其中，此方法就是从声明标识中获取用户显示名称。

## 方法
位于Microsoft.SharePoint.Utilities.SPUtility类，具体描述如下：
``` c#
//从基于声明的身份验证标识信息中获取用户显示名
public static string GetDisplayName(string userName);
```
## 使用场景
在SharePoint开发过程中，我们希望在页面某个地方显示当前登录用户的显示名称，而不是一串奇怪的声明标识字符。此时我们就可以使用此方法：

```c#
string displayName = Microsoft.SharePoint.Utilities.SPUtility.GetDisplayName(SPContext.Current.Web.CurrentUser.LoginName)；
```

## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 在Powershell代码中运行此代码，返回结果：
# sp\spadmin
#####
[Microsoft.SharePoint.Utilities.SPUtility]::GetDisplayName("i:0#.w|sp\spadmin");
```
