# 页面访问授权的秘密：SPUtility.EnsureAuthentication

## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
确定当前用户是否有SharePoint网站的访问授权，如果没有，则会抛出UnauthorizedAccessException的异常。

## 方法
位于Microsoft.SharePoint.Utilities.SPUtility类，具体描述如下：
``` c#
//确定当前用户是否有当前网站的访问授权
public static void EnsureAuthentication();
//确定当前用户是否有某个指定网站的访问授权
public static void EnsureAuthentication(SPWeb web);
```


## 使用场景
在需要确保当前用户针对某个网站是否有访问授权的时候就可以使用此方法。

比如在`Microsoft.SharePoint.WebControls.UnsecuredLayoutsPageBase`类中就有使用此方法的代码：

``` c#
protected override void OnLoad(EventArgs e)
{
    this.StopRequestIfClientIsNotValid();
    //如果页面不允许匿名访问，则需要验证当前用户是否有访问授权
    if (!this.AllowAnonymousAccess)
    {
        SPUtility.EnsureAuthentication();
    }
    if (!this.ShowStandardControls)
    {
        this.HideCommonControls();
    }
    base.OnLoad(e);
}


```

`UnsecuredLayoutsPageBase`类是自定义应用程序页面的一个可以使用的基类，主要功能是可以配置应用程序页面是否需要身份认证。比如做Form认证的登录页面，就可以使用此基类配置可以匿名访问。


## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 在Powershell代码中运行此代码，会得到一个异常：
# 尝试执行未经授权的操作
# 这是因为在Powershell进程中获取不到HttpContext.Current对象
#####
[Microsoft.SharePoint.Utilities.SPUtility]::EnsureAuthentication();
```

如上示例，此方法的最近测试场景是放在应用程序页面中。
