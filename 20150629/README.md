# 合并Url：SPUtility.ConcatUrls
    作者：杨柳@水杉网络


## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
在SharePoint服务器端对象模型中，Site Url，资源的ServerRelativeUrl是分开存储的，因此，在开发过程中如果需要得到SharePoint某个资源的完整资源路径，往往需要做字符串拼装，此方法就是为了方便我们进行路径拼装。

## 方法
位于Microsoft.SharePoint.Utilities.SPUtility类，具体描述如下：
``` c#
//拼装Url路径，目的只是确保两个参数直接连接部分只能有一个"/"连接符。
public static string ConcatUrls(string firstPart, string secondPart)
{
    return SPUrlUtility.CombineUrl(firstPart, secondPart);
}
//此方法来至于SPUrlUtility类
public static string CombineUrl(string baseUrlPath, string additionalNodes)
{
    if (baseUrlPath == null)
    {
        return additionalNodes;
    }
    if (additionalNodes == null)
    {
        return additionalNodes;
    }
    if (baseUrlPath.EndsWith("/", StringComparison.OrdinalIgnoreCase))
    {
        if (additionalNodes.StartsWith("/", StringComparison.OrdinalIgnoreCase))
        {
            baseUrlPath = baseUrlPath.TrimEnd(new char[] { '/' });
        }
        return (baseUrlPath + additionalNodes);
    }
    if (additionalNodes.StartsWith("/", StringComparison.OrdinalIgnoreCase))
    {
        return (baseUrlPath + additionalNodes);
    }
    return (baseUrlPath + "/" + additionalNodes);
}




```


## 使用场景
此方法的目的在于确保Url的两部分字符串连接的时候只能出现一个"/"符号，正如前面的介绍，这是由于SharePoint资源Url存储的特殊性决定的。比如下面截取乐`Microsoft.SharePoint.ProjectPolicyUtility`类中使用此方法的方式：

``` c#
//获取列表 PackageList 的服务器端相对路径
internal static SPList GetPackageList(SPSite site)
{
    SPWeb rootWeb = site.RootWeb;
    string strUrl = SPUtility.ConcatUrls(rootWeb.ServerRelativeUrl, "/Lists/PackageList");
    try
    {
        return rootWeb.GetList(strUrl);
    }
    catch (FileNotFoundException)
    {
        return null;
    }
}



```

仔细阅读此方法的实现代码，目的很简单，就是为了确保Url路径拼装过程中接口处只有一个“/”符号，在实际开发过程中，可以根据业务逻辑扩展此方法。


## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 输出结果：http://www.shuishan-tech.com/about.aspx
#####
[Microsoft.SharePoint.Utilities.SPUtility]::ConcatUrls("http://www.shuishan-tech.com/", "/about.aspx");

#####
# 输出结果：http://www.shuishan-tech.com/about.aspx
#####
[Microsoft.SharePoint.Utilities.SPUtility]::ConcatUrls("http://www.shuishan-tech.com", "/about.aspx");

#####
# 输出结果：http://www.shuishan-tech.com/about.aspx
#####
[Microsoft.SharePoint.Utilities.SPUtility]::ConcatUrls("http://www.shuishan-tech.com/", "about.aspx");

#####
# 输出结果：http://www.shuishan-tech.com/about.aspx
#####
[Microsoft.SharePoint.Utilities.SPUtility]::ConcatUrls("http://www.shuishan-tech.com", "about.aspx");
```
