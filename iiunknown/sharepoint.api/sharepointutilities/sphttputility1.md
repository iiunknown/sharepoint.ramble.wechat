# SPHttpUtility.HtmlEncode

## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
HtmlEncode字面上很好理解：HTML字符编码，在各个语言平台上也都有对应支持的工具类，比如在C#平台上就有：

```c#
//将字符串转化为HTML编码的字符串
System.Web.HttpUtility.HtmlEncode(string value);
```
SharePoint平台上提供的工具类SPHttpUtility中也提供了HtmlEncode的方法，在SharePoint开发中该如何使用呢？

## 方法
在Microsoft.SharePoint.Utilities.SPHttpUtility类下，有多个HtmlEncode的重载方法，具体描述如下：
``` c#
//将GUID转换为注册表格式大写的字符串
public static string HtmlEncode(Guid valueToEncode);
//将Int型转化为不依赖区域性的字符串
public static string HtmlEncode(int valueToEncode);
//将Html格式的字符串转化为安置HTML格式编码后的字符串
public static string HtmlEncode(string valueToEncode);
//将Html格式的字符串转化为安置HTML格式编码后的字符串，并使用System.IO.TextWriter输出流返回
public static void HtmlEncode(string valueToEncode, TextWriter output);
```
## 使用场景
给HTML控件的的值或者属性赋值时，对于值字符串的内容无法预知的情况下，则需要使用此方法对字符串进行转义，防止字符串中包含HTML特殊字符而影响整个页面布局。

在SharePoint内部代码中截取的典型应用场景，位于` Microsoft.SharePoint.WebControls.RichTextField`类中

```c#
protected override void RenderFieldForInput(HtmlTextWriter output)
{
    //...
    if ((base.textBox != null) && !string.IsNullOrEmpty(direction))
    {
        base.textBox.Attributes["dir"] = SPHttpUtility.HtmlEncode(direction);
    }
    //...
}

```



## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 格式化带HTML标签的文本。
# 输出结果：&lt;div&gt;hello SharePoint.&lt;/div&gt;
#####
[Microsoft.SharePoint.Utilities.SPHttpUtility]::HtmlEncode("<div>hello SharePoint.</div>");
#####
# 格式化Guid实例。
# 输出结果：与{BE2CC461-ECF3-4153-B1FF-0DB8589ABA4B}一致格式的注册表格式大写字符串。
#####
[Microsoft.SharePoint.Utilities.SPHttpUtility]::HtmlEncode([System.Guid]::NewGuid());

```
