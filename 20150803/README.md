# 判断是否是全路径Url：SPUrlUtility.IsUrlFull
    作者：杨柳@水杉网络


## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
SharePoint是一个Web应用平台，所以，SharePoint平台的绝大部分资源都可以使用Url来进行描述。对于外界提供的一个Url路径，有可能是全路径，也有可能是相对路径。因此在开发过程中我们需要很好地甄别。

## 方法
位于Microsoft.SharePoint.Utilities.SPUrlUtility类，具体描述如下：
``` c#
//判断是否是全路径Url
public static bool IsUrlFull(string url)
{
    Uri uri;
    return Uri.TryCreate(url, UriKind.Absolute, out uri);
}


```


## 使用场景
在某些尽量需要全路径资源文件引用的情况下，可以使用此方法对资源文件路径进行校验，比如在SharePoint中，引入到页面的CSS资源文件路径，SharePoint就会做全路径的检测。

下面截取了`Microsoft.SharePoint.WebControls.CssRegistrationRecord`类中使用此方法的方式：

``` c#
//验证CSS引用地址是否是全路径，否则创造全路径CSS引用地址。
public string CanonicalCssReference
{
    get
    {
        if (this.m_CanonicalRef == null)
        {
            SPContext current = SPContext.Current;
            if (SPUrlUtility.IsUrlFull(this.CssReference))
            {
                this.m_CanonicalRef = this.CssReference;
            }
            else
            {
                try
                {
                    this.m_CanonicalRef = CssLink.MakeCssUrl(current.Web, 0, this.CssReference, current.IsDesignTime);
                }
                catch (SPException)
                {
                    return "";
                }
            }
        }
        return this.m_CanonicalRef;
    }
}
```

## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 输出结果：False
#####
[Microsoft.SharePoint.Utilities.SPUrlUtility]::IsUrlFull("abc");

#####
# 输出结果：True
#####
[Microsoft.SharePoint.Utilities.SPUrlUtility]::IsUrlFull("http://abc");

```
