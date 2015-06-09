# Ajax.NET for SharePoint

    作者：杨柳@水杉网络

## Ajax.NET介绍
[Ajax.NET](http://ajaxpro.codeplex.com/)是微软开发体系下优秀的Ajax框架，虽然不是轻量级的前端解决方案，但是完美结合了微软开发体系，在服务器端很容易注册Ajax方法，客户端使用也很方便。

## 问题
Ajax.NET在.NET的反射机制基础上提供的Ajax解决方案，引入到SharePoint项目中后，发现无法在Ajax.NET的后台生命周期中使用SPContext.Curent。这是由于反射机制引起的，客户端的每一次Ajax请求通过Ajax.NET服务器端解析后，Ajax.NET在Web应用根路径直接反射对应的服务器端方法，所以，SPContext.Current为空。常见的解决办法是在SharePoint中引用原生的Ajax.NET，每次客户端请求过来的数据需要携带SiteId，WebId，ListId，ItemId等一系列参数，使用这些参数在服务器端方法中重新构造SPSite，SPWeb，SPList，SPListItem实例，然后再处理业务代码。这是一条可行的路，而且我自己很长时间也在这么使用。但是一旦上了大规模的应用，对Ajax依赖较多，这样的处理方式就显得繁琐，于是冒出想让Ajax.NET支持SharePoint的想法。在Ajax.NET基础上修改后，我们已经在产品和项目中使用了一年多的时间，考虑到Ajax.NET本身来至于开源社区，不敢独享，也分享出来给有需要的同学。

## 解决方案
1. 修改RegisterTypeForAjax方法代码

	AjaxPro.Utilities.RegisterTypeForAjax方法是用来注册服务器端代码类的类型，使用此方法注册后类型后，Ajax.NET会输出一份脚本文件到客户端，此脚本文件的名称使用的是类型的强名称，比如：

	```
	http://*/ajaxpro/Ajax.Net.Demo.AjaxWebPart,%20Ajax.Net.Demo,%20Version=1.0.0.0,%20Culture=neutral,%20PublicKeyToken=62b55bddef13ac12.ashx
	```
	其实问题的根源就在这里，http://*/ajaxpro/不是一个有效的SharePoint地址，所以SPContext不能被自动构造。因此，第一步，在此注册方法里为Url增加必须的参数，将Url为：

	```
	http://*/ajaxpro/Ajax.Net.Demo.AjaxWebPart,%20Ajax.Net.Demo,%20Version=1.0.0.0,%20Culture=neutral,%20PublicKeyToken=62b55bddef13ac12.ashx?siteid=a0c534dd-1a69-4b90-95cb-d5af37adca18&webid=f076a79d-e110-40d3-83db-c4d4accf43b5&listid=5e6c47db-ed26-4c53-8725-1cf0eabbe546&itemid=26&lcid=2052&formmode=1
	```
	这个步骤准备好了服务器端构造SPContext必要的参数，在客户端使用此脚本时就有机会从QueryString里获取参数重构SPContext。
2. 修改TypeJavaScriptHandler中的ProcessRequest方法代码

	TypeJavaScriptHandler.ProcessRequest方法主要是根据注册的服务器端类型及方法生成对应的客户端脚本，并且输出到客户端，以便客户端在脚本中调用。下面是此方法生成的脚本文件例子：

	<script src="https://code.csdn.net/snippets/591780.js"></script>

	其中```url```属性后的querystring是干预后的结果，和第一步一样，加入了我们需要的必要参数。

3. 修改AjaxHandlerFactory中的GetHandler方法代码

	AjaxHandlerFactory.GetHandler方法是服务器端每次处理Ajax请求的方法，因此，重构SPContext的任务就在这里。具体代码如下：

	<script src="https://code.csdn.net/snippets/591771.js"></script>

## 版本说明

* Ajax.Net：9.2.17.1
* .Net Framework 4.5
* SharePoint 2013

## 示例
* 原代码地址：[Ajax.NET for SharePoint](https://github.com/shuishan-tech/ajax.net)
* 示例代码地址：[Demo](https://github.com/shuishan-tech/ajax.net/tree/master/Demo/Ajax.NET.Demo)
