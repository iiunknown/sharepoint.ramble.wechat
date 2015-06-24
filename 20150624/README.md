#LINQ to SharePoint
    作者：柒月
玩过`Entity Framework`的小伙伴们应该都比较熟悉LINQ了，在SharePoint中，也可以使用LINQ对SPList进行查询。
##步骤
1. 使用[SPMetal](https://msdn.microsoft.com/en-us/library/ee538255(office.14).aspx "SPMetal")导出SPList实体模型(cs文件)。默认情况下，此工具会将SPWeb级别下的所有列表、文档库等内容导出成实体模型。也可以通过[编写XML](https://msdn.microsoft.com/en-us/library/ee535056(v=office.14).aspx "编写XML")，使其只生成指定的内容。
2. 将步骤1中的文件添加到VS项目中，并添加引用`Microsoft.SharePoint.Linq`。

完成以上两步，就可以使用LINQ查询SPList了。例：
``` C#     
using (DemoSiteDataContext context = new DemoSiteDataContext(SPContext.Current.Web.Url))
{
   var result = from p in context.TESTLIST
                where p.标题 != "AAA"
                select p;
                //var result2 = context.TESTLIST.Where(p => p.标题 != "AAA");   //Lambda表达式
                List<TESTLIST项目> list = result.ToList();
}
```
##关心的几个问题
- ###效率和可用性

  LINQ to SharePoint真正去SPList中取数据之前，实际上是将LINQ表达式或Lambda表达式转化成了CAML。所以，在一般情况下，性能应该是良好的。当然也有意外，当表达式[无法转化CAML](https://msdn.microsoft.com/en-us/library/ee536585.aspx)，或者无法准确地转化成CAML时，异常和性能问题也将会出现。

  例：
``` C#
var results = from projectItem in context.PriorityProjects
              where projectItem.ExecutiveSponsor.Equals(sponsor) //string.Equals无法转化成<Eq>标签，这句表达式将会跳过此筛选条件而获取所有数据，再对结果集进行LINQ to Objects筛选，最终返回数据
             //where projectItem.ExecutiveSponsor == sponsor    //‘==’符号可以转化成<Eq>标签，将会转成带此标签的CAML对数据源进行筛选
              select projectItem;
```


- ###权限提升（`SPSecurity.RunWithElevatedPrivileges`）

  LINQ to SharePoint无法直接使用权限提升获取数据，对此，[Kaneboy大神给出了解决方法和解释](http://www.cnblogs.com/kaneboy/archive/2012/01/25/2437086.html)。
  

- ###连接查询

  LINQ to SharePoint使用连接查询必须要借助Lookup字段。返回结果集时，若要包含Lookup表中的其他字段，只能用新的数据类型来包装结果集。

例：
``` C#
  dataContext.Customers.Select(c=>c.Orders).ToArray(); //异常。 Orders是Lookup表，想要返回Lookup表，用CAML语句是没办法直接表达的

  dataContext.Customers.Select(c => new { Description =  c.Order.Description, CustomerId = c.Order.CustomerId }).ToArray(); //此处将返回的结果包装成新的匿名类型，可以正确获取查询结果
```
