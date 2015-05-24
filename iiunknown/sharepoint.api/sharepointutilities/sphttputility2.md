# SPHttpUtility.HtmlDecode

## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
HtmlDecode是HtmlEncode的逆向方法，将编码过的字符还原为Html字符。

## 方法
在Microsoft.SharePoint.Utilities.SPHttpUtility类下，有两个个HtmlDecode的重载方法，具体描述如下：
``` c#
//将编码后的字符串转化为Html字符串。
public static string HtmlDecode(string valueToDecode);
//将编码后的字符串转化为Html字符串，是否将'&nbsp'字符转化为空格。
public static string HtmlDecode(string valueToDecode, bool decodeNbsp);
```
## 使用场景
将格式化后的Html字符串转换为标准的Html字符串，格式化后得Html字符串便于存储和传输，通过HtmlDecode方法，可以重新还原为Html字符串，用于Html格式输出。


## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 格式化后的Html字符串转换为Html格式字符串。
# 输出结果：<div>hello SharePoint.</div>
#####
[Microsoft.SharePoint.Utilities.SPHttpUtility]::HtmlDecode("&lt;div&gt;hello SharePoint.&lt;/div&gt;");
#####
# 格式化后的Html字符串转换为Html格式字符串。
# 输出结果：<div>hello&nbspSharePoint.</div>
#####
[Microsoft.SharePoint.Utilities.SPHttpUtility]::HtmlDecode("&lt;div&gt;hello&nbspSharePoint.&lt;/div&gt;", $false);

```
