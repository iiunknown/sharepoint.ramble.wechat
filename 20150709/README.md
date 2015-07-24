#几个开源的CAML生成类
	作者：柒月
之前，我们介绍了[LINQ to SharePoint](https://github.com/iiunknown/sharepoint.ramble.wechat/blob/master/20150624/README.md)。

本着“我不造轮子，我只做轮子的搬运工”的人生目标，接下来，我们一起看一看几位大神编写的CAML生成类。同样，它使开发者无需关心CAML语句如何编写，而更专注于业务逻辑的实现，并且看起来更加轻巧、灵活。

###CAML.NET

引用：[http://camldotnet.codeplex.com/](http://camldotnet.codeplex.com/)

例：
``` C#
string typeName = "My Content Type";
string simpleQuery =
    CAML.Query(
        CAML.Where(
            CAML.Or(
                CAML.Eq(
                    CAML.FieldRef("ContentType"), 
                    CAML.Value(typeName)),
                CAML.IsNotNull(
                    CAML.FieldRef("Description")))),
        CAML.GroupBy(
            true,
            CAML.FieldRef("Title",CAML.SortType.Descending)),
        CAML.OrderBy(
            CAML.FieldRef("_Author"),
            CAML.FieldRef("AuthoringDate"),
            CAML.FieldRef("AssignedTo",CAML.SortType.Ascending))
    );
```
生成的CAML：
``` CAML
<Query>
   <Where>
      <Or>
         <Eq>
            <FieldRef Name="ContentType" />
            <Value Type="Text">My Content Type</Value>
         </Eq>
         <IsNotNull>
            <FieldRef Name="Description" />
         </IsNotNull>
      </Or>
   </Where>
   <GroupBy Collapse="TRUE">
      <FieldRef Ascending="FALSE" Name="Title" />
   </GroupBy>
   <OrderBy>
      <FieldRef Name="_Author" />
      <FieldRef Name="AuthoringDate" />
      <FieldRef Ascending="TRUE" Name="AssignedTo" />
   </OrderBy>
</Query>
```
###Camlex.NET

引用：[http://camlex.codeplex.com/](http://camlex.codeplex.com/)

例：
``` C#
var caml =
    Camlex.Query()
        .Where(x => (int)x["ProductID"] == 1000 && ((bool)x["IsCompleted"] == false || x["IsCompleted"] == null))
            .ToString();
```
生成的CAML：
``` CAML
<Where>
  <And>
    <Eq>
      <FieldRef Name="ProductID" />
      <Value Type="Integer">1000</Value>
    </Eq>
    <Or>
      <Eq>
        <FieldRef Name="IsCompleted" />
        <Value Type="Boolean">0</Value>
      </Eq>
      <IsNull>
        <FieldRef Name="IsCompleted" />
      </IsNull>
    </Or>
  </And>
</Where>
```

###Caml Query

引用：[https://camlquery.codeplex.com/](https://camlquery.codeplex.com/)

例：
``` C#
SPSite site = new SPSite("http://jyserver:81");

SPList list = site.RootWeb.Lists"Notice";

QueryField field1 = new QueryField("标题", false); //the second parameter explain if the first parameter is a internal name.
//or : QueryField field1 = new QueryField("Title"); //"Title" is internal name. 
//DateTime type field
TypedQueryField<DateTime> field2 = new TypedQueryField<DateTime>("Expires");

CamlExpression expr = field1.Contains("Test1") && field2 >= DateTime.Now.AddDays(-1);
SPListItemCollection items =
ListQuery
.From(list)
.Where(expr)
.GetItems();
```
生成的CAML:
``` CAML
<Where>
  <And>
    <Contains>
      <FieldRef Name="标题" />
      <Value Type="Text">Test1</Value>
    </Contains>
    <Geq>
      <FieldRef Name="Expires" />
      <Value IncludeTimeValue="TRUE" Type="DateTime">2015-07-08T21:43:45Z</Value>
    </Geq>
  </And>
</Where>
```

###CAML Builder

引用：[http://blog.163.com/chinaren_bjr/blog/static/16989137201251010335793/](http://blog.163.com/chinaren_bjr/blog/static/16989137201251010335793/)

例：
``` C#
Query = CAMLBuilder.BuildQuery(
        new CAMLBuilder.WhereBuilder("Title", "Text", "测试", OperationSymbol.BeginsWith).And("Author", "User", "张三")), 
        new CAMLBuilder.OrderBuilder("Created", false))
```
生成的CAML:
``` CAML
<Where>
  <And>
    <BeginsWith>
      <FieldRef Name="Title" />
       <Value Type="Text">测试</Value>
    </BeginsWith>
    <Eq> 
      <FieldRef Name="Author" />
       <Value Type="User">张三</Value>
    </Eq>
  </And>
</Where>
```
