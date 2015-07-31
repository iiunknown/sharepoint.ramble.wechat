# 判断是否合法文件名：SPUrlUtility.IsLegalFileName
    作者：杨柳@水杉网络


## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
文档库是SharePoint的重要功能之一，几乎贯穿了整个SharePoint十多年的生命周期。文档库的基本功能就是保存文件，对于上传到文档库的文件名一定会做合规检查，那么在开发过程中SharePoint提供了这样的方法供我们使用。

## 方法
位于Microsoft.SharePoint.Utilities.SPUrlUtility类，具体描述如下：
``` c#
//判断是否合法文件名
public static bool IsLegalFileName(string name)
{
    return IsLegalFileName(name, false);
}

```


## 使用场景
基于SharePoint文档库提供文档管理的解决方案，很多时候会自己提供上传文件的处理，这种情况下，需要先判断用户提供的文件名是否合法，让人机交互更加顺滑。

比如下面截取了`Microsoft.SharePoint.SPFieldFile`类中使用此方法的方式：

``` c#
//验证SPFieldFile类型实例的值是否是有效文件名
public override object ValidateAndParseValue(SPListItem item, string value)
{
    //...
    if (!string.IsNullOrEmpty(str))
    {
        if (!SPUrlUtility.IsLegalFileName(str))
        {
            throw new SPFieldValidationException(SPResource.GetString(CultureInfo.CurrentUICulture, "InvalidFileOrFolderName", new object[] { str }));
        }
        if (SPUtility.EndsWithAspNetSuffix(str))
        {
            throw new SPFieldValidationException(SPResource.GetString(CultureInfo.CurrentUICulture, "NoAspNetSuffix", new object[] { str }));
        }
    }
    return value;
}

```

## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 输出结果：True
#####
[Microsoft.SharePoint.Utilities.SPUrlUtility]::IsLegalFileName("abc.txt");

#####
# 输出结果：False
#####
[Microsoft.SharePoint.Utilities.SPUrlUtility]::IsLegalFileName("~abc.txt");

```
