# 使用Schema定义推送列表模板及实例注意事项
    作者：杨柳@水杉网络


## 摘要
在使用Visual Studio进行SharePoint开发过程中，经常会使用场解决方案包含Feature推送列表模板及其实例定义（Schema定义）。Visual Studio提供的SharePoint开发工具虽然谈不上特别方便易用，但对于列表字段定义来讲也比在SharePoint 列表管理中一个个增加来得方便，而且也方便项目的部署迁移。

但是，在使用Schema定义过程中，也有需要值得注意的地方，一步小心就会掉入陷阱中。此篇主要介绍两个注意点：
* 管好你的ListTemplate.Type
* 拯救Lookup字段

## 管好你的ListTemplate.Type

### 描述
首先我们看一个标准的列表模板定义：

```xml
<ListTemplate
        Name="SSW_Workflow"
        Type="10028"
        BaseType="0"
        OnQuickLaunch="TRUE"
        SecurityBits="11"
        Sequence="410"
        DisplayName="SSW_Workflow"
        Description="我的列表定义"
        Image="/_layouts/15/images/itgen.png"/>
```
注意到其中的`Type`属性，值为数字型。如果使用Visual Studio创建，在同一个解决方案范围内，一般会在10000的基础上会自动增长，在同一个解决方案包里，所有的ListTemplate定义的`Type`属性不允许重复，在Feature激活的时候会报错。这一切看上去自然、和谐。那如果是两个不同的解决方案呢，甚至是不同团队开发的解决方案包，在同样地网站上激活，会是怎样一种状况？

    项目组一开发了一个解决方案包，其中推送了一个列表模板和实例叫做`List1`，按照Visual Studio的默认做法，这个列表模板的Type="10000"。

    项目组二开发了一个解决方案包，其中推送了一个列表模板和实例叫做`List2`，同理，默认情况下Visual Studio也会设置列表模板Type="10000"。

分别推送两个解决方案包，会发现`List1`，`List2`列表模板及实例都好端端的推送到了网站，需要的东西一个不少，常见的功能也都完好无损。那么，题目上说的***管好你的ListTemplate.Type***是在危言耸听？

### 问题在哪儿？
这真的不是谣言，也不是威胁，只是在某些特定的状况下才会引爆，下面我们就来点导火索。

SharePoint的列表及列表项事件也是开发过程中的常用的功能，一般用于数据同步，校验，分发等。这些事件也是可以通过Schema定义来推送的，例如：

```xml
<Receivers ListTemplateId="10000">
      <Receiver>
        <Name>SSWorkflowTaskEventReceiverItemUpdated</Name>
        <Type>ItemUpdated</Type>
        <Assembly>$SharePoint.Project.AssemblyFullName$</Assembly>
        <Class>ShuiShan.S2.Workflow.SSWorkflowTaskEventReceiver</Class>
        <SequenceNumber>10000</SequenceNumber>
        <Synchronization>Asynchronous</Synchronization>
      </Receiver>
  </Receivers>
```
定义了一个列表项更新完成后的异步事件，如果使用Visual Studio来定义，会让你选择一个列表模板或者列表实例，`ListTemplateId`自动关联上你所选择的，这里的`ListTemplateId`就是在列表模板中定义的Type属性值。看到这里你是否察觉到了啥？是的，不同的解决方案包推送上去的列表模板会有相同的Type属性值，如果事件接收器绑定列表模板ID，那么这两个列表内容的变化都会触发这个事件，而实际情况往往是只想侦听自己推送的列表。

问题发掘出来了，如何解决呢？

### 解决方案

1. 如果只是针对某一个列表进行侦听，那么自行修改Visual Studio生成的Schema定义，使用ListUrl属性指定到唯一的列表：

    ```xml
    <Receivers ListUrl="Lists/SSW_WorkflowTask">
          <Receiver>
            <Name>SSWorkflowTaskEventReceiverItemUpdated</Name>
            <Type>ItemUpdated</Type>
            <Assembly>$SharePoint.Project.AssemblyFullName$</Assembly>
            <Class>ShuiShan.S2.Workflow.SSWorkflowTaskEventReceiver</Class>
            <SequenceNumber>10000</SequenceNumber>
            <Synchronization>Asynchronous</Synchronization>
          </Receiver>
      </Receivers>
    ```

2. 如果是需要针对列表模板下所有的列表进行侦听，那真的就是——管好你的ListTemplate.Type，保证其唯一性。特别是在跨不同解决方案包的情况下，作为网站管理员要好好把关。

## 拯救Lookup字段
此篇这两个小标题看起来都有浓浓的英雄主义气息，特别是此节的小标题，看上去大事儿不好。故事要从这里说起：

### 描述
上节描述了使用Schema定义列表模板及实例进行推送，轻描淡写的一个过程，其中辛酸只有过来人才知道。这个过程中最重要的一件事情就是添加字段定义。虽然Visual Studio有一个可视化的界面用于字段定义，但是还不够，看过SPField Schema定义的同学就知道，里面有计其数的属性也算是琳琅满目，简单地可视化界面无法支撑繁杂的列表字段定义。比如Lookup字段的定义：

```xml
<Field Name="Department3" ID="{da6cd449-5b55-4bd2-8803-d52aa061d8f9}" DisplayName="$Resources:ShuiShan.S2.Workflow,SSWField_Department3" Type="Lookup" List="lists/SSO_Department" ShowField="Title" Group="ShuiShan.S2.Workflow" Description="" />
```
其中属性`ShowField`, `List`从可视化界面上无从下手。当然，这点儿小问题还谈不上拯救，那——

### 问题在哪儿？
我们先分析一下上面Lookup字段的标准定义属性：
* ShowField：Lookup字段显示内容的InternalName
* List：Lookup字段关联的列表

无论是SharePoint项目还是产品开发过程中，我们不能承诺一次就能将列表定义完全定义好，随着功能和需求的变化，列表字段也会跟随变化，比如还需要往现有列表上增加一个新的Lookup字段。和平常的开发一样，我们打开Visual Studio，在列表定义上增加需要的新Lookup字段，打包，部署。如果这么做过的同学都知道，这个时候如果列表实例没有删除新建（很多时候不能删除重建，比如之前的列表实例已经存在业务数据。），那么新增加的Lookup字段从列表管理页面上查看不会显示所关联的主列表，从代码中去获取这个字段会抛出异常。为什么会出现这个问题？我们看看SharePoint针对Lookup字段定义中的`List`属性描述：

    可选属性，类型为 Text。用于标识查阅字段 (Type="Lookup") 的目标列表。
    如果目标列表已存在，List 属性的值应是标识目标列表的 GUID 的字符串表示（包括括号）。如果目标列表与该字段所属的列表相同，则可以指定"自身"。
    如果目标列表不存在，List 属性的值可以是相对于网站的 URL（例如"Lists/My List"），但只有在创建目标列表与创建查阅字段所用的功能相同时才适用。在这种情况下，Field 元素上 List 属性的值必须等于创建目标列表的 ListInstance 元素上的 Url 属性的值。

这下真相大白了，如果是已经推送过的列表实例，属于已经存在的列表，所以`List`属性需要使用列表实例的Guid。恩，拍脑袋一想：可以，找到列表实例Guid，写上了，不就OK了？再想想，如果到另外一个新的环境部署呢，`List`属性需要写相对路径而不是Guid，这是个解不开的死结。

好，那我们曲线救国：那就不使用Visual Studio推送了，手工通过SharePoint列表管理一个个加吧，这么做看上去没有问题，但如果项目够大，或者产品有很多客户使用，我们怎么去统一这个手工过程（我曾经深陷入手工同步的苦海不能自拔，一个异常查很久发现是Lookup字段没有推送正确）？

### 解决方案
仔细研究了SharePoint推送的过程，发现如果是解决方案包第一次推送，使用相对Url这种定义，SharePoint自己会在列表实例生成好了之后为Lookup字段使用列表实例的Guid，而第二次推送的时候SharePoint不会检查Schema定义的差异，而Lookup主列表实例已经存在，没有做列表实例生成这一步，所以Guid就没有，它的推送程序把Lookup字段定义上的`List`属性值直接付给了SPLookupField上的LookupList属性，而SPLookupField.LookupList只能是Guid。所以解决方法的思路就是:

    在列表定义的Feature激活后扫描所有的列表定义，找出Lookup类型字段，寻找列表实例Guid，使用代码干涉Lookup字段实例。具体内容阅读下面的代码。

1. Feature激活事件中的代码

    ``` c#
    public class ListDefinitionEventReceiver : SPFeatureReceiver
    {
        // 取消对以下方法的注释，以便处理激活某个功能后引发的事件。

        public override void FeatureActivated(SPFeatureReceiverProperties properties)
        {
            SPWeb web = properties.Feature.Parent as SPWeb;
            SPElementDefinitionCollection c = properties.Definition.GetElementDefinitions(System.Globalization.CultureInfo.CurrentUICulture);
            //根据Feature里的ListTemplate定义更新列表字段，主要是以下两个方面的更新：
            //1. 在已有列表实例上增加Lookup字段，默认情况下属性中的List定义使用ListUrl，这种情况下增加的Lookup字段异常，必须使用LookupList的GUID替换属性中的List值。
            //2. 在第一次并非使用列表定义推送的列表实例上增加列表字段定义里的新增字段。
            try
            {
                ListFieldsUpgrade g = new ListFieldsUpgrade(web, properties.Definition);
                g.Upgrade();
            }
            catch (Exception ex)
            {
                Utilities.Utility.Log("ShuiShan.S2.Workflow.ListDefinitionEventReceiver.FeatureActivated upgrade list field definition error: {0}", ex.Message);
            }
        }
    }
    ```

2. 列表字段升级处理类

    ``` c#

    namespace ShuiShan.S2.Administration
    {
        /// <summary>
        /// 列表字段定义升级的处理类
        /// </summary>
        public class ListFieldsUpgrade
        {
            private Dictionary<string, SPElementDefinition> _ListTemplates = new Dictionary<string, SPElementDefinition>();
            private Dictionary<string, SPElementDefinition> _ListInstances = new Dictionary<string, SPElementDefinition>();
            private Dictionary<string, SPElementDefinition> _ListFields = new Dictionary<string, SPElementDefinition>();

            private SPFeatureDefinition _Definition;
            private SPWeb _Web;

            public ListFieldsUpgrade(SPWeb web, SPFeatureDefinition definition)
            {
                _Web = web;
                _Definition = definition;
            }
            /// <summary>
            /// 处理列表定义数据
            /// </summary>
            private void EnsureData()
            {
                if (_Definition == null)
                {
                    Utilities.Utility.Log("ShuiShan.S2.Administration.ListFieldsUpgrade.EnsureData: SPFeatureDefinition is null.");
                    return;
                }
                SPElementDefinitionCollection dc = _Definition.GetElementDefinitions(System.Globalization.CultureInfo.CurrentUICulture);
                if (dc == null || dc.Count < 1)
                {
                    Utilities.Utility.Log("ShuiShan.S2.Administration.ListFieldsUpgrade.EnsureData: SPElementDefinitionCollection is null or empty.");
                    return;
                }

                #region 扫描Feature中所有的ListTemplate和ListInstance定义，分类存储供下一步使用。
                //ListTemple：使用<Type, SPElementDefinition>键值存储
                //ListInstance：使用<Url.ToLower(), SPElementDefinition>键值存储
                foreach (SPElementDefinition d in dc)
                {
                    if (d.ElementType == "ListTemplate")
                    {
                        string templateType = d.XmlDefinition.Attributes["Type"].Value;
                        templateType = templateType.Trim();
                        if (string.IsNullOrEmpty(templateType.Trim())) continue;
                        if (_ListInstances.ContainsKey(templateType)) continue;

                        _ListTemplates.Add(templateType, d);

                    }
                    else if (d.ElementType == "ListInstance")
                    {
                        string listUrl = d.XmlDefinition.Attributes["Url"].Value;
                        listUrl = listUrl.Trim();
                        if (string.IsNullOrEmpty(listUrl)) continue;
                        listUrl = listUrl.ToLower();
                        if (_ListInstances.ContainsKey(listUrl)) continue;

                        _ListInstances.Add(listUrl, d);
                    }
                    //处理网站字段定义
                    else if (d.ElementType == "Field")
                    {
                        string fieldName = d.XmlDefinition.Attributes["Name"].Value;
                        if (string.IsNullOrEmpty(fieldName)) continue;
                        _ListFields.Add(fieldName, d);
                    }
                }
                #endregion

                #region 处理ListInstance字段
                foreach (KeyValuePair<string, SPElementDefinition> listInstance in _ListInstances)
                {
                    string listUrl = listInstance.Key;
                    string templateType = listInstance.Value.XmlDefinition.Attributes["TemplateType"].Value.Trim();
                    if (!_ListTemplates.ContainsKey(templateType)) continue;

                    SPElementDefinition d = _ListTemplates[templateType];
                    string templateName = d.XmlDefinition.Attributes["Name"].Value.Trim();
                    if (string.IsNullOrEmpty(templateName)) continue;

                    Stream stream = d.FeatureDefinition.GetFile(string.Format("{0}\\Schema.xml", templateName));
                    using (XmlReader rdr = XmlReader.Create(stream))
                    {
                        rdr.ReadToDescendant("List");
                        SPList list = Utilities.Utility.GetList(_Web, listUrl);
                        if (list == null) continue;
                        rdr.ReadToDescendant("Fields");
                        using (XmlReader innerRd = rdr.ReadSubtree())
                        {
                            innerRd.ReadToDescendant("Field");
                            do
                            {
                                if (innerRd.Name != "Field") break;
                                //Field Name
                                string fieldName = innerRd.GetAttribute("Name");
                                //Field Type
                                string fieldType = innerRd.GetAttribute("Type");
                                //Field List, 在字段为Lookup的情况下才会设置，必须在ReadOuterXml之前读取，否则文件流指针往下移动后读取不到。
                                string lookupListUrl = innerRd.GetAttribute("List");
                                if (string.IsNullOrEmpty(fieldName)) continue;
                                if (list.Fields.ContainsField(fieldName))
                                {
                                    #region 扫描处理处理列表实例创建后推送的Lookup类型字段
                                    //通过Module定义的列表实例，第一次推送成功后，之后再次添加Lookup字段，Lookup字段定义上的List属性必须使用GUID作为值，这么做和第一次推送使用ListUrl作为值就有冲突，因此，在Module定义的时候统一使用ListUrl作为值，在这里判断当前列表的Lookup字段的List属性是否是GUID，如果不是，则在网站中寻找LookupList实例，获取它的ID更新Lookup字段的List属性。
                                    if (fieldType == "Lookup" || fieldType == "LookupMulti")
                                    {
                                        //如果Lookup字段定义上的List属性值没有，定义不完整，则继续下一个Field的处理。
                                        if (string.IsNullOrEmpty(lookupListUrl)) continue;
                                        SPField field = list.Fields.GetField(fieldName);
                                        SPList lookupList = Utilities.Utility.GetList(_Web, lookupListUrl);
                                        if (field is SPFieldLookup)
                                        {
                                            SPFieldLookup lookupField = field as SPFieldLookup;
                                            Guid g = Guid.Empty;
                                            if (!Guid.TryParse(lookupField.LookupList, out g))
                                            {
                                                //通过反射SPField上的SetFieldAttributeValue方法，设置List属性为空值，这样再为SPLookupField.LookupList赋值时就不会产生异常，下面是SPLookupField.LookupList set方法的代码。
                                                /*
                                                set
                                                {
                                                    if (!string.IsNullOrEmpty(base.GetFieldAttributeValue("List")) && !(base.GetFieldAttributeValue("List") == value))
                                                    {
                                                        throw new SPException(SPResource.GetString(System.Globalization.CultureInfo.CurrentCulture, "CannotChangeLookupList", new object[0]));
                                                    }
                                                    this.lookupListSet = true;
                                                    this.lookupList = value;
                                                    base.SetFieldAttributeValue("List", value);
                                                }
                                                 */
                                                try
                                                {
                                                    Type t = typeof(SPField);
                                                    MethodInfo mInfo = t.GetMethod("SetFieldAttributeValue", BindingFlags.NonPublic | BindingFlags.Instance);
                                                    mInfo.Invoke(lookupField, new object[] { "List", "" });

                                                    lookupField.LookupList = lookupList.ID.ToString();
                                                    lookupField.Update();
                                                    Utilities.Utility.Log("ShuiShan.S2.Administration.ListFieldsUpgrade.EnsureData update LookupField successed: List[{0}] FieldName[{1}] LookupList[{2}]", listUrl, fieldName, lookupListUrl);
                                                }
                                                catch (Exception ex)
                                                {
                                                    Utilities.Utility.Log("ShuiShan.S2.Administration.ListFieldsUpgrade.EnsureData update LookupField error: List[{0}] FieldName[{1}] LookupList[{2}] Exception[{3}]", listUrl, fieldName, lookupListUrl, ex.Message);
                                                }
                                            }
                                        }
                                    }
                                    #endregion
                                }
                                else
                                {
                                    #region 处理列表模版中存在而列表实例中不存在的字段
                                    string fieldXml = innerRd.ReadOuterXml();

                                    if (fieldType == "Lookup" || fieldType == "LookupMulti")
                                    {
                                        //如果Lookup字段定义上的List属性值没有，定义不完整，则继续下一个Field的处理。
                                        if (string.IsNullOrEmpty(lookupListUrl)) continue;
                                        SPList lookupList = Utilities.Utility.GetList(_Web, lookupListUrl);
                                        if (lookupList != null)
                                        {
                                            Regex r = new Regex("List=\\S*");
                                            fieldXml = r.Replace(fieldXml, string.Format("List=\"{0}\"", lookupList.ID));
                                        }
                                    }
                                    try
                                    {
                                        list.Fields.AddFieldAsXml(fieldXml, false, SPAddFieldOptions.AddFieldInternalNameHint);
                                        Utilities.Utility.Log("ShuiShan.S2.Administration.ListFieldsUpgrade.EnsureData Add SPFeild successed: List[{0}] FieldName[{1}] LookupList[{2}] SchemaXml[{3}]", listUrl, fieldName, lookupListUrl, fieldXml);
                                    }
                                    catch (Exception ex)
                                    {
                                        Utilities.Utility.Log("ShuiShan.S2.Administration.ListFieldsUpgrade.EnsureData Add SPFeild error: List[{0}] FieldName[{1}] LookupList[{2}] SchemaXml[{3}] Exception[{4}]", listUrl, fieldName, lookupListUrl, fieldXml, ex.Message);
                                    }
                                    continue;
                                    #endregion
                                }
                            } while (innerRd.ReadToFollowing("Field"));
                        }
                        list.Update();
                    }
                }
                #endregion

                #region 处理网站字段
                foreach (KeyValuePair<string, SPElementDefinition> listField in _ListFields)
                {
                    string fieldName = listField.Key;
                    string lookupListUrl = listField.Value.XmlDefinition.Attributes["List"].Value;
                    string fieldType = listField.Value.XmlDefinition.Attributes["Type"].Value;
                    if (string.IsNullOrEmpty(fieldType)) continue;
                    if (!(fieldType == "Lookup" || fieldType == "LookupMulti")) continue;
                    SPField field = _Web.Fields.GetField(fieldName);
                    if (field == null || !(field is SPFieldLookup)) continue;
                    SPList lookupList = Utilities.Utility.GetList(_Web, lookupListUrl);
                    SPFieldLookup lookupField = field as SPFieldLookup;
                    Guid g = Guid.Empty;
                    if (!Guid.TryParse(lookupField.LookupList, out g))
                    {
                        try
                        {
                            Type t = typeof(SPField);
                            MethodInfo mInfo = t.GetMethod("SetFieldAttributeValue", BindingFlags.NonPublic | BindingFlags.Instance);
                            mInfo.Invoke(lookupField, new object[] { "List", "" });

                            lookupField.LookupList = lookupList.ID.ToString();
                            lookupField.Update();
                            Utilities.Utility.Log("ShuiShan.S2.Administration.ListFieldsUpgrade.EnsureData update Web LookupField successed: FieldName[{0}] LookupList[{1}]", fieldName, lookupListUrl);
                        }
                        catch (Exception ex)
                        {
                            Utilities.Utility.Log("ShuiShan.S2.Administration.ListFieldsUpgrade.EnsureData update Web LookupField error: FieldName[{0}] LookupList[{1}] Exception[{2}]", fieldName, lookupListUrl, ex.Message);
                        }
                    }

                }
                #endregion
            }

            /// <summary>
            /// 根据Feature定义升级列表字段
            /// </summary>
            public void Upgrade()
            {
                EnsureData();
            }
        }
    }

    ```

