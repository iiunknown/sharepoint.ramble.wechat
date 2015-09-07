# 维护SharePoint Web应用程序的Web.config文件

    杨柳@水杉网络
    
##介绍

先，重复一个观点：SharePoint是ASP.NET最大的产品实践，没有之一。说到ASP.NET，除了开发方面的控件，页面生命周期，MVC等等一系列的知识点涌上心头，在配置管理方面第一反应该是Web.config文件。对于SharePoint来讲，Web.config同样很重要，除了基本的ASP.NET所需的配置项，还放置了很多SharePoint的配置项，同时，如果做SharePoint的自定义开发，也有可能需要对Web.config文件进行修改。SharePoint作为ASP.NET基础上的产品，且提供了多服务器场的拓扑管理结构，所以SharePoint提供了针对Web.config文件进行修改的方法。此篇就聊聊在SharePoint中维护Web.config文件的几种方式。


## Web.config文件位置

Web.config文件包含在文件系统中的以下文件夹中：

* ***\\Inetpub\wwwroot\wss\VirtualDirectories\ 端口号*** ```SharePoint 内容 Web 应用程序定义配置设置的 web.config 文件。```
* ***\\Inetpub\wwwroot\wss\VirtualDirectories\ 管理中心的端口号*** ```SharePoint 管理中心应用程序定义配置设置的 web.config 文件。```
* ***\\Inetpub\wwwroot\wss\VirtualDirectories\ 端口号 \wpresources*** ```在 Web 应用程序的 Web 部件资源中使用的 web.config 文件。```
* ***\\Program Files\Common Files\Microsoft Shared\Web Server Extensions\wpresources*** ```在全局程序集缓存的 Web 部件资源中使用的 web.config 文件。```
* ***%ProgramFiles%\Common Files\Microsoft Shared\web server extensions\14 | 15 | 16\CONFIG*** ```共同定义用于扩展其他 Web 应用程序的配置设置的 web.config 文件和其他 .config 文件。```
* ***%ProgramFiles%\Common Files\Microsoft Shared\web server extensions\14 | 15 | 16\ISAPI*** ```为 /_vti_bin 虚拟目录定义配置设置的 web.config 文件。```
* ***%ProgramFiles%\Common Files\Microsoft Shared\web server extensions\14 | 15 | 16\TEMPLATE\LAYOUTS*** ```为 /_layouts 虚拟目录定义配置设置的 web.config 文件。```

        注意：本节中的`14 | 15 | 16`文件夹路径根据SharePoint版本确定，SharePoint 2010对应14，SharePoint 2013对应15， SharePoint 2016对应16。

## Web.config文件管理方式

熟悉ASP.NET开发的同学，对于复杂度不高的项目，Web.config文件的维护很多时候可以依赖手工完成，为什么到SharePoint上，却提供了对象模型，配置管理这样的方式呢？原因有下面两点：
* 首先，SharePoint作为一个产品，会定期发布更新或者Service Pack，甚至安装升级到下一个版本，这些操作很有可能会覆盖Web.config文件。
* SharePoint的可扩展拓扑结构是产品本身的优势之一，因此在稍微复杂的应用场景下，一个服务器场往往会涉及多个服务器，甚至会涉及到多个服务器场，因此手工维护Web.config文件工作量大，容易产生疏忽。

### 以编程方式添加或者删除Web.config设置
通过编程的方式添加或者删除Web.config中的设置，需要使用到`Microsoft.SharePoint.Administration`命名空间中的`SPWebConfigModification`类，此类的实例相当于一个XPath路径及以下内容的描述对象。

#### 示例一： 添加Web.config配置项
```c#
SPWebService service = SPWebService.ContentService;

SPWebConfigModification myModification = new SPWebConfigModification();
myModification.Path = "configuration/SharePoint/SafeControls";
myModification.Name = "SafeControl[@Assembly='MyCustomAssembly'][@Namespace='MyCustomNamespace'][@TypeName='*'][@Safe='True']";
myModification.Sequence = 0;
myModification.Owner = "User Name";
myModification.Type = SPWebConfigModification.SPWebConfigModificationType.EnsureChildNode;
myModification.Value = "<SafeControl Assembly='MyCustomAssembly' Namespace='MyCustomNamespace' TypeName='*' Safe='True' />";
service.WebConfigModifications.Add(myModification);
 
/*Call Update and ApplyWebConfigModifications to save changes*/ 
service.Update();
service.ApplyWebConfigModifications();
```

#### 示例二：删除Web.config中的配置项
```c#
SPWebConfigModification configModFound = null;
SPWebApplication webApplication = SPWebApplication.Lookup(new Uri("http://localhost/"));
Collection<SPWebConfigModification> modsCollection = webApplication.WebConfigModifications;

// Find the most recent modification of a specified owner
int modsCount1 = modsCollection.Count;
for (int i = modsCount1 - 1; i > -1; i--)
{
    if (modsCollection[i].Owner == "User Name")
    {
        configModFound = modsCollection[i];
    }
}

// Remove it and save the change to the configuration database  
modsCollection.Remove(configModFound);
webApplication.Update();

// Reapply all the configuration modifications
webApplication.Farm.Services.GetValue<SPWebService>().ApplyWebConfigModifications();
```

#### 注意事项
* 调用SPWebService.ApplyWebCoinfigModifications()方法，其实是交给一个计时器作业去执行，所以并不是即使生效，会有一定时间差。
* 需要有系统管理员权限才能执行代码的权限。
* 示例一中的方式会影响整个服务器场的Web应用程序，如果只是需要修改特定的Web应用程序，可以使用`oWebSite.Site.WebApplication.WebConfigModifications.Add(MyModification) `方式增加config配置对象到指定的Web应用程序。

## 使用补充文件方式
在第一节`Web.config文件位置`中介绍了SharePoint相关的Web.config文件位置，其中一个是`%ProgramFiles%\Common Files\Microsoft Shared\web server extensions\14 | 15 | 16\CONFIG`。打开这个文件夹，发现其中除了Web.config文件外，还包含一些webconfig.\*.xml文件。这些`webconfig.\*.xml`文件称之为Web.config补充文件。补充文件的内容示例：

```xml
<actions>
   <add path="configuration/SharePoint/SafeControls">
      <SafeControl
         Assembly="System.Web, Version=1.0.5000.0, Culture=neutral, 
            PublicKeyToken=b03f5f7f11d50a3a"
         Namespace="System.Web.UI.WebControls"
         TypeName="*"
         Safe="True"/>
   </add>
   <remove path="configuration/SharePoint/RuntimeFilter"/>
   <add path="configuration/SharePoint">
      <RuntimeFilter
         Assembly="Company.Product, Version=1.0.1000.0, 
            Culture=neutral, PublickKeyToken=1111111111"
         Class="MyRuntTimeFilter",
         BuilderUrl="MyBuilderUrl"/>
   </add>
</actions>
```

在创建Web应用程序或者扩展Web应用程序时，SharePoint会以Web.config文件为基础，再合并所有webconfig.\*.xml文件内容到Web.config文件中，作为新创建或者扩展出来的Web应用程序的Web.config文件。

上面讲到的是一种机制，补充文件本身该如何管理呢？最直白的，既然是文件系统物理文件，而且xml文本，因此可以直接手工维护。但这不是我们的初衷，因为这解决不了多服务器场带来的工作量，因此，下面介绍一下使用场解决方案包推送补充文件。

#### 使用场解决方案管理补充文件
当一个SharePoint自定义场解决方案项目需要维护Web.conifig内容时，优先使用此方法。
1. 在VS中创建场解决方案（适用于SharePoint 2010及以上版本）。
2. 在解决方案中映射`CONFIG`文件夹，并创建需要的补充文件。
    ![](001.png)
3. 打包安装解决方案包后，对应服务器的`CONFIG`目录下就会被推送对应的补充文件。

        注意：补充文件的命令格式说明，请参考MSDN，这里不做累述。

## 结尾
上面介绍的两种方式都是偏重于开发，且各有优劣，总结一下使用过程中的几个注意事项：
* 如果只希望精确控制某个Web应用程序，适合使用编程模式。
* 如果希望控制场中所有Web应用程序，两种方式都适合。
* 使用补充文件方式，如果是先创建了Web应用程序，再推送的解决方案包，补充文件不会生效，必须运行`Install-SPApplicationContent` Powershell命令才会生效。
