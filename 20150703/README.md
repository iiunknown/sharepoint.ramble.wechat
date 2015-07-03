# 友好显示字节长度：SPUtility.FormatSize
    作者：杨柳@水杉网络

## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
在SharePoint开发过程中，如果使用过SPFile.Length属性，应该知道这个值表示一个文档的字节数，显示给用户的时候，我们希望提供给用户一个更加利于理解的单位，比如KB，MB，GB等。此方法就是帮助我们把一个long数组转换为友好可读字节长度格式。

## 方法
位于Microsoft.SharePoint.Utilities.SPUtility类，具体描述如下：
``` c#
//格式化long数组为可读字节长度格式。
public static string FormatSize(long cbSize)
{
    if (cbSize > 0x400L)
    {
        double num;
        if (cbSize > 0x40000000L)
        {
            num = Math.Round((double) (Convert.ToDouble(cbSize) / 1073741824.0), 1);
            return SPResource.GetString("FileSizeGB", new object[] { num.ToString() });
        }
        if (cbSize > 0x100000L)
        {
            num = Math.Round((double) (Convert.ToDouble(cbSize) / 1048576.0), 1);
            return SPResource.GetString("FileSizeMB", new object[] { num.ToString() });
        }
        num = Math.Round((double) (Convert.ToDouble(cbSize) / 1024.0), 1);
        return SPResource.GetString("FileSizeKB", new object[] { num.ToString() });
    }
    if (cbSize > 0L)
    {
        return SPResource.GetString("FileSizeSmall", new object[0]);
    }
    return SPResource.GetString("FileSizeKB", new object[] { "0" });
}

```


## 使用场景
正如上面的介绍，获取一个SharePoint文档长度后，需要将数值转换为可读格式，此时就可以使用此方法。


## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 输出结果：< 1 KB
#####
[Microsoft.SharePoint.Utilities.SPUtility]::FormatS
ize(1024)

#####
# 输出结果：1 KB
#####
[Microsoft.SharePoint.Utilities.SPUtility]::FormatS
ize(1024 + 1)

#####
# 输出结果：1024 KB
#####
[Microsoft.SharePoint.Utilities.SPUtility]::FormatS
ize(1024 * 1024)

#####
# 输出结果：1 MB
#####
[Microsoft.SharePoint.Utilities.SPUtility]::FormatS
ize(1024 * 1024 + 1)
```
