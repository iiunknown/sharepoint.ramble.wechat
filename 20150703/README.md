# 格式化日期时间：SPUtility.FormatDate
    作者：杨柳

## 基本信息
程序集
> Microsoft.SharePoint.dll

命名空间
> Microsoft.SharePoint.Utilities

类型
> 静态方法

## 介绍
在SharePoint中，有自己特别的日期时间格式：SPDateFormat，此方法就是把System.DateTime系统时间转换为SPDateFormat格式的字符串。

## 方法
位于Microsoft.SharePoint.Utilities.SPUtility类，具体描述如下：
``` c#
//格式化System.DateTime为SPDateFormat格式字符串。
public static string FormatDate(SPWeb web, DateTime date, SPDateFormat fmt)
{
    return FormatDate(web, date, fmt, true);
}
//供上一个方法调用，此方法多传递一个参数toWebLocalTime：是否转换为SPWeb本地时间。
internal static string FormatDate(SPWeb web, DateTime date, SPDateFormat fmt, bool toWebLocalTime)
{
    SPContext context = SPContext.GetContext(web);
    return FormatDate(web, date, (SPCalendarType) context.RegionalSettings.CalendarType, fmt, toWebLocalTime);
}
//真正落地执行的方法，相对于Public的方法来讲，多了SPCalendarType和toWebLocalTime参数。
private static string FormatDate(SPContext context, DateTime date, SPCalendarType calendarType, SPDateFormat fmt, bool toWebLocalTime)
{
    if (context == null)
    {
        throw new ArgumentNullException("context");
    }
    if (toWebLocalTime)
    {
        date = context.RegionalSettings.TimeZone.UTCToLocalTime(date);
    }
    SimpleDate di = new SimpleDate(date.Year, date.Month, date.Day);
    if (SPCalendarType.Gregorian != calendarType)
    {
        int jDay = SPIntlCal.LocalToJulianDay(SPCalendarType.Gregorian, ref di);
        SPIntlCal.JulianDayToLocal(calendarType, jDay, ref di);
    }
    if (fmt != SPDateFormat.MonthDayOnly)
    {
        return context.RegionalSettings.DateOptions.GetDowLongDateString(di);
    }
    if ((context.Web != null) && (context.Web.WebTemplateId == 9))
    {
        context.RegionalSettings.DateOptions.MonthDayPattern = (context.Web.UIVersion == 4) ? "MMMM dd" : "MMM dd";
    }
    return context.RegionalSettings.DateOptions.GetMonthDayDateString(di);
}
```


## 使用场景
SharePoint在数据库中存储的时间为UTC格式，所以在获取使用过程中，有可能需要转换为本地时间，另外更多地需求就是按照SharePoint特定场景规定的格式显示（SPDateForm）。

比如下面截取了`Microsoft.SharePoint.SPFieldDateTime`类中使用此方法的方式：

``` c#
//转换SPFieldDateTime字段值为SPDateFormat格式文本
public static string GetFieldValueAsText(DateTime data, SPWeb web, SPDateFormat dateformat)
{
    return SPUtility.FormatDate(web, data, dateformat);
}

```


## 测试代码
> 以下代码在SharePoint Powershell命令行工具中执行

``` powershell
# 根据实际情况替换WebUrl
$web = Get-SPWeb http://loalhost
# 当前系统时间
$d = [System.DateTime]::Now
#####
# 输出结果：2015/7/2 23:04
# 注意，输出结果受$d变量和当前SharePoint环境影响。
#####
[Microsoft.SharePoint.Utilities.SPUtility]::FormatDate($web, $d, [Microsoft.SharePoint.Utilities.SPDateFormat]::DateTime);

#####
# 输出结果：2015/7/2
# 注意，输出结果受$d变量和当前SharePoint环境影响。
#####
[Microsoft.SharePoint.Utilities.SPUtility]::FormatDate($web, $d, [Microsoft.SharePoint.Utilities.SPDateFormat]::DateOnly);

#####
# 输出结果：23:06
# 注意，输出结果受$d变量和当前SharePoint环境影响。
#####
[Microsoft.SharePoint.Utilities.SPUtility]::FormatDate($web, $d, [Microsoft.SharePoint.Utilities.SPDateFormat]::TimeOnly);

#####
# 输出结果：2015-07-02T23:07:27Z
# 注意，输出结果受$d变量和当前SharePoint环境影响。
#####
[Microsoft.SharePoint.Utilities.SPUtility]::FormatDate($web, $d, [Microsoft.SharePoint.Utilities.SPDateFormat]::ISO8601);

#####
# 输出结果：7月2日
# 注意，输出结果受$d变量和当前SharePoint环境影响。
#####
[Microsoft.SharePoint.Utilities.SPUtility]::FormatDate($web, $d, [Microsoft.SharePoint.Utilities.SPDateFormat]::MonthDayOnly);

#####
# 输出结果：2015年7月
# 注意，输出结果受$d变量和当前SharePoint环境影响。
#####
[Microsoft.SharePoint.Utilities.SPUtility]::FormatDate($web, $d, [Microsoft.SharePoint.Utilities.SPDateFormat]::MonthYearOnly);
```
