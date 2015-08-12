# 设置当前访问线程的语言：SPUtility.SetThreadCulture
    作者：杨柳@水杉网络


## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
SharePoint依赖ASP.NET的多语言机制，提供了多语言模式。在自定义开发过程中，有时候不依赖SharePoint本身的多语言切换机制，而是想自定义多语言切换机制，这个时候，可以考虑在后台使用此方法。

## 方法
位于Microsoft.SharePoint.Utilities.SPUtility类，具体描述如下：
``` c#
//设置当前线程的语言
public static void SetThreadCulture(CultureInfo culture, CultureInfo uiCulture)
{
    Utility.SetThreadCulture(culture, uiCulture, true);
}
```


## 使用场景
在特定情况下，需要获取SharePoint中某种特别的语言资源，可以先使用此方法设置线程的语言环境，然后获取资源，则可以获取对应语言的资源。

下面截取了`Microsoft.SharePoint.Utilities.ThreadCultureScope`类中使用此方法的方式：

``` c#
//构造函数中使用此方法，将当前线程语言记住，然后使用传递过来的语言设置线程语言。
    public ThreadCultureScope(CultureInfo culture, CultureInfo uiCulture)
    {
        this.m_culture = Thread.CurrentThread.CurrentCulture;
        this.m_uiCulture = Thread.CurrentThread.CurrentUICulture;
        SPUtility.SetThreadCulture(culture, uiCulture);
    }

```
