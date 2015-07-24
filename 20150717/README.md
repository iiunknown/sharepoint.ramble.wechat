# 判断传出电子邮件是否配置：SPUtility.IsEmailServerSet
    作者：杨柳@水杉网络

## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍

此工具方法判断当前服务器场是否配置过传出电子邮件。在实际开发中，有可能会使用传出电子邮件服务发送邮件，此方法用于判断当前服务器场的传出电子邮件服务是否可用，以便给用户或者系统管理员更明确的信息。

## 方法
位于Microsoft.SharePoint.Utilities.SPUtility类，具体描述如下：
``` c#
// 提供SPWeb实例，判断当前服务器场传出电子邮件服务是否可用。
public static bool IsEmailServerSet(SPWeb web)
{
    if (((web == null) || (web.Site == null)) || (web.Site.WebApplication == null))
    {
        throw new ArgumentNullException("web");
    }
    return web.Site.WebApplication.IsEmailServerSet;
}

```

注意，不要被上面方法传递的SPWeb实例参数所迷惑，认为判断电子邮件服务和SPWeb有关系，做过SharePoint管理的同学应该都知道，一个服务器场只有一个传出电子邮件的配置地方，继续往下看，探探究竟。

跟踪`WebApplication.IsEmailServerSet`代码，就能看到究竟：

``` c#
// 获取WebApplication上关联的OutboundMailService实例，以此服务器地址作为是否设置过电子邮件服务器的标志。
internal bool IsEmailServerSet
{
    get
    {
        SPOutboundMailServiceInstance outboundMailServiceInstance = this.OutboundMailServiceInstance;
        return ((outboundMailServiceInstance != null) && !string.IsNullOrEmpty(outboundMailServiceInstance.Server.Address));
    }
}
```



## 使用场景

SharePoint内部使用此方法来判断电子邮件服务是否可用，比如在发送分享文档的提示邮件时：

``` c#
// Microsoft.SharePoint.Sharing.SPDocumentSharingManager.SendEmailNotifications
private static void SendEmailNotifications(SPWeb web, SPSecurableObject secObject, List<SPUser> resolvedUsers, List<string> externalEmailAddresses, string customMessage, string anonymousLink)
{
    if (SPUtility.IsEmailServerSet(web))
    {
        CachedSenderInformation senderInfo = new CachedSenderInformation();
        SPSharingEmailHelper.SendResourceIsAvailableNotification(web, secObject, customMessage, resolvedUsers, externalEmailAddresses, anonymousLink, null, senderInfo);
    }
    // ...
}

```
在自定义开发过程中，如果需要使用传出电子邮件服务，可以考虑在发送电子邮件时使用此方法判断服务是否可用，给用户一个更加准确的信息反馈。

## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 输出结果：True
# 输出结果视当前SharePoint传出电子邮件配置而定
#####
$web = Get-SPWeb http://demo.shuishan-tech.com
[Microsoft.SharePoint.Utilities.SPUtility]::IsEmailServerSet($web)

```
