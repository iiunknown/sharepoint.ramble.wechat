# SPUtility.CreateISO8601DateTimeFromSystemDateTime

## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
将一个系统时间转换为ISO8601时间格式的字符串（yyyy-mm-ddThh:mm:ssZ）。

## 方法
位于Microsoft.SharePoint.Utilities.SPUtility类，具体描述如下：
``` c#
public static string CreateISO8601DateTimeFromSystemDateTime(DateTime dtValue)
{
    StringBuilder builder = new StringBuilder();
    builder.Append(dtValue.Year.ToString("0000"));
    builder.Append("-");
    builder.Append(dtValue.Month.ToString("00"));
    builder.Append("-");
    builder.Append(dtValue.Day.ToString("00"));
    builder.Append("T");
    builder.Append(dtValue.Hour.ToString("00"));
    builder.Append(":");
    builder.Append(dtValue.Minute.ToString("00"));
    builder.Append(":");
    builder.Append(dtValue.Second.ToString("00"));
    builder.Append("Z");
    return builder.ToString();
}
```
下面是维基百科对日期时间组合在ISO8601中格式的描述：

    合并表示时，要在时间前面加一大写字母T，如要表示北京时间2004年5月3日下午5点30分8秒，可以写成2004-05-03T17:30:08+08:00或20040503T173008+08。

## 使用场景
最常见的使用场景是在SPQuery的CAML中针对日期时间字段进行查询。一般会出现两个容易忽视的地方：
* 查询时没有使用UTC标准时间，导致时区之间的差异。
* 查询语句中没有使用ISO 8601格式，导致根据特定日期时间查询异常。

第二种情况就是使用此篇介绍的API去解决，在CAML语句中使用此方法格式化后的字符串作为日期时间字符串。

``` xml
<Query>
   <Where>
      <And>
         <Gt>
            <FieldRef Name='Modified' />
            <Value Type='DateTime'>2015-05-29T12:00:00Z</Value>
         </Gt>
         <Lt>
            <FieldRef Name='Modified' />
            <Value Type='DateTime'>2015-05-29T10:00:00Z</Value>
         </Lt>
      </And>
   </Where>
</Query>
```



## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
#####
# 格式化后的Html字符串转换为Html格式字符串。
# 输出结果：2015-05-29T10:00:00Z (运行时环境当前的时间)
#####
[Microsoft.SharePoint.Utilities.SPUtility]::CreateISO8601DateTimeFromSystemDateTime([System.DateTime]::Now);
```
