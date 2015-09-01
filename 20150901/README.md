#使用Module（模块）推送文件至SharePoint
	作者：柒月
Module像是一个容器，负责将包含其中的文件一一推送至指定的目录。

在项目中，我们可以按照需要推送的文件的类型，创建各种Module，如：负责推送母版页的Module，负责推送布局页的Module，负责推送文件至指定文档库的Module等。

###SharePoint项目中添加一个Module
1. 在Visual Studio中，打开或创建一个SharePoint项目
2. 右键SharePoint项目->添加->新建项->模块（英文版为`Module`），点击“添加”按钮。此处会创建一个新的Module，默认名为`Module1`
3. 删除`Module1`下默认的`Sample.txt`文件。删除此文件后，也会在删除`Module1`下`Elements.xml`内相应的定义
4. 可以在`Module1`下创建文件夹，部署时，也将会创建相应的目录结构，并将文件推送至指定文件夹下
5. `Module1`或其下的文件夹中，添加->现有项，或者添加->新建项->`文本文件`，会自动在`Elements.xml`中创建相应的定义
6. 部署SharePoint项目，`Module1`中包含的文件，会按照`Elements.xml`的定义，推送至指定目录

###Elements.xml的结构
以下是一个Module中的Elements.xml内容：
```
<?xml version="1.0" encoding="utf-8"?>
<Elements xmlns="http://schemas.microsoft.com/sharepoint/">
  <Module Name="MasterPages" Url="_catalogs/masterpage" RootWebOnly="TRUE">
    <File Path="MasterPages/Normal.master" Url="Custom/Normal.master"  Type="GhostableInLibrary" ReplaceContent="True">
      <Property Name="Title" Value="母版页-常规" />
    </File>
  </Module>
</Elements>
```

- `Module`节点。`Name`指定此模块的名称；`Url`指定此模块部署的相对路径；`RootWebOnly`指定此模块是否只推送至根网站。详见：[Module Element](https://msdn.microsoft.com/en-us/library/office/ms434127.aspx)
- `File`节点。`Path`描述此文件的相对路径（相对于`%ProgramFiles%\Common Files\Microsoft Shared\web server extensions\15\TEMPLATE\Features\Feature`）；`Url`相对于`Module Url`的相对路径；`Type`指定此文件的部署类型，当指定为`GhostableInLibrary`时，表示此文件将作为文档库的Item；`ReplaceContent`表示，若文件已被应用或占用，是否强制重新部署。详见：[File Element](https://msdn.microsoft.com/en-us/library/office/ms459213.aspx)
- `Property`节点。`Name`和`Value`描述其父级`File`节点的自定义属性。详见[Property Element](https://msdn.microsoft.com/en-us/library/office/cc264281.aspx)



参考：[How to: Include Files by Using a Module](https://msdn.microsoft.com/en-us/library/ee231524(v=vs.120).aspx)