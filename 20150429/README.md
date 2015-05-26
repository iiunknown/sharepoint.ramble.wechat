# 浅析SPField的DisplayName，InternalName，StaticName
    作者：杨柳@水杉网络

参与过SharePoint开发的朋友多少都接触过SPField——SharePoint字段。SPField可以算作是SharePoint逻辑结构体系中最小元素（从场，网站集，网站，列表，字段一路下来），是构建SharePoint信息元素的砖块儿，是支撑SharePoint表单信息呈现最重要的SharePoint元素（没有之一）。看似这么小巧玲珑的一个对象，如果你仔细阅读过MSDN上的定义或者反射SharePoint代码阅读过，会发现它大有来头：

 - 是SharePoint字段元素定义、值存取机制、UI层控件展现三位一体的中枢。

> 从SharePoint 2010开始，我成功干扰过这个中枢：使用SharePoint原生的字段元素定义，值存取机制，使用现代的前端框架替换掉SharePoint基于ASP.NET 2.0的UI层控件体系。接下来有机会再深入聊聊这个话题。

 - 基于SPField分发出去一共有41个字段类（以SharePoint Foundation 2013为例），这是整个SharePoint表单构建的基础，我们常见的文本字段，数字字段，查阅项字段等无一例外全部构建于此。

谈到这里，我们仿佛从一个小窗口挤进SharePoint浩瀚世界，但是，这里我们不准备在这个浩瀚世界遨游，只信手拈来其中一个微小的角落：介绍Field Name相关属性的使用和注意事项。在日常的开发过程中，我们会发现获取一个列表的字段可以通过`DisplayName`，`InternalName`，`StaticName`获取，那么，它们是谁？究竟该如何正确使用它们？接下来我们从定义，开发角度看看。


## 使用Feature推送SPField定义
如果你阅读过[MSDN][1]关于SharePoint Features Schemas相关的说明，会发现SPField定义从属于Features schemas下，也就是说，在Feature中推送中可以定义SPField。从官方的定义可以看到，一个XML节点元素需要支持所有类型的SPField定义需求，所以Field Element属性看上去十分庞杂。但是，我们今天的主题是*Name，所以我把和主题相关的属性筛选出来进行说明：
```xml
<Field
    DisplayName="Text"
    Name="Text"
    StaticName="Text"
    Title="Text"
    ...>
</Field>
```
1. `DisplayName`: 从字面上就能理解到是显示名称，属性值为文本类型，支持`$Resources:String`资源引用，也就是说，字段的显示名称实现多语言轻轻松松。
2. `Name`：对应服务器端模型`InternalName`属性。注意，无论是通过Schema推送还是代码创建或者UI创建字段，`InternalName`一旦被SharePoint系统接受就不可更改，作为字段在系统中的唯一身份ID，而且，在首次赋值过程中，SharePoint很有可能会修改你的初始值。比如我们输入的中文，SharePoint会转换为Unicode，如果我们定义的名字系统中已经存在，SharePoint会跟随一个数字角标用以区别。另外一个需要注意的地方是`InternalName`在SharePoint系统中最大字符长度为32位，也就是说过长的`Name`属性定义也会被SharePoint截取并添加数字角标。
3. `StaticName`: 也是字段内部名称的一种，和上一个`Name`属性不一样的是，在同样一个容器中（列表，网站）允许重名。之所以这么做，是因为`InternalName`为了保障唯一性可能会被SharePoint修改，而`StaticName`严格保存定义者的定义，从服务器端API也提供了方法保证我们能顺利获取自己定义的字段。在Schema定义中`StaticName`是非必填项，如果没有设置值，SharePoint会使用`InternalName`填充它。
4. `Title`: 这个属性能出现在Schema中让我感到诧异，因为即使在定义中赋值了，也不会显示出来，。`Title`和`DisplayName`其实是好基友，它一出生就和`DisplayName`绑定在一起，有代码为证：

``` c#
public string Title
    {
        get
        {
            //...
            sPFieldDisplayName = this.GetFieldAttributeValue("DisplayName", 3);
            //...
        }
        set
        {
            //...
            this.SetFieldAttributeValue("DisplayName", value);
            //...
        }
    }

```

> 以上代码截取至Microsoft.SharePoint.SPField，为了节省排版空间，只显示了关键代码，如果有需要，请阅读SharePoint原生代码。

使用Feature Schema推送SPField定义需要注意以下几点：
1. `Name`属性尽量使用英文，这样可以增加推送成功后`InternalName`的可读性。
2. 在多项目复杂的网站环境下，Field `Name`属性定义最好加上自有的特殊前缀，减少重复可能性，避免在开发过程中因为`InternalName`被SharePoint修改后带来的不便。
3. 如果第二项实在做不到，那么就一定要使用`StaticName`定义，而且一定要遵守第二项中的唯一前缀规则，这样在开发过程中可以得心应手获取字段定义。

## 开发过程中SPField.*Name使用
上一节介绍了使用Feature Schema推送SPField定义时*Name的定义及推送过程中的注意事项。其中也提到了在开发过程中使用的规则。这里详细介绍开发过程中如何使用第一节中定义的*Name属性来获取字段，以及在使用过程中的注意事项。
根据SPField.*Name获取SPField实例的方法存在于`SPFieldCollection`集合对象实例中，一般说来这个集合对象实例位于以下几个对象实例中：

 - SPWeb
 - SPList
 - SPListItem
 - SPContentType

具体方法介绍如下：

``` c#
/*
根据InternalName, DisplayName, StaticName的顺序搜寻字段实例，如果从这三个属性都没有定位到，则抛出异常。
*/
SPFieldCollection.GetField(string name);
/*
顾名思义，根据InternalName获取字段实例，因为SharePoint保障InternalName具有唯一性，所以在知道InternalName情况下推荐直接使用此方法。如果没有找到对应的字段，会抛出异常。
*/
SPFieldCollection.GetFieldByInternalName(string name);
/*
这个方法更加顾名思义：通过StaticName搜寻字段实例，找不到也不会抛出异常，只会返回Null值（多么体贴的行为，其实GetFieldByInternalName也有类似的方法，只不过访问限制符为internal...不明白SharePoint产品组为什么不开放此方法）。
*/
SPFieldCollection.TryGetFieldByStaticName(string name);
```

在实际开发过程中的注意事项，想来想去就这么一条最重要的：使用`GetField`， `GetFieldByInternalName`这两个方法很有可能会引发异常，所以在使用前推荐使用`ContainsField`方法判断想要获取的字段是否存在于集合中。

## 尾声
SharePoint是Web应用平台，SharePoint是ASP.NET最大的产品实践，这是我一直以来的观点。今天再增加一条：SPField构建起SharePoint整个表单系统。
由小及大，我们通过SPField去了解SharePoint的Schema定义，数据存取方式，UI呈现规则，进而去了解整个SharePoint系统的运行规则，最后可以进一步去探索Web世界的秘密。这无疑是在当下行业中Web炙手可热状态下我们可以行之有效的一条学习路径。
没有前进，也没有后退，那都是方向。

  [1]: https://msdn.microsoft.com/en-us/library/office/ee539685%28v=office.15%29.aspx
