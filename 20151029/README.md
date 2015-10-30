# SharePoint上下文——SPContext
	作者：柒月

在SharePoint项目开发过程中，SPContext给我们提供了很多的便利。我们常用SPContext.Current.Web来获取当前SPWeb对象，以及隐藏在SPWeb之下的各种信息。但也别忽略了，SPContext中还有一些其他有意思的属性，合理利用好它们，等于走了捷径。:)

###注意事项
- EventReceiver和TimerJob中，SPContext.Current为null
- 使用SPFarm管理员登录时，SPContext.Current.Web.CurrentUser为系统账户（SHAREPOINT\system），此时用HttpContext.Current.User.Identity.Name可获得当前登录名

###常用属性
<table>
   <tr>
      <td><b>属性</b></td>
      <td><b>应用场景</b></td>
   </tr>
   <tr>
      <td>SPSite</td>
      <td>获取当前网站集对象<br/>SPSite.RootWeb：获取当前网站集的根网站</td>
   </tr>
   <tr>
      <td>SPWeb</td>
      <td>获取当前Web对象<br/>SPWeb.CurrentUser：获取当前登录SPUser对象</td>
   </tr>
   <tr>
      <td>ViewContext</td>
      <td>在列表视图页面时，可获取SPViewContext对象<br/>ViewContext.View.ViewFields：获取当前视图下的所有字段</td>
   </tr>
   <tr>
      <td>SPListItem</td>
      <td>在列表项的新增、修改、查看页面时，可以获取当前SPListItem对象<br/>SPListItem[fieldName]：访问或赋值列表项字段<br/>SPListItem.Update()：更新列表项</td>
   </tr>
   <tr>
      <td>SPFile</td>
      <td>若当前页面是页面库下的页面，则将当前页面作为SPFile对象返回<br/>SPFile.ServerRelativeUrl：当前页面文件的相对路径<br/>SPFile.Publish(comments)：发布当前页面</td>
   </tr>
   <tr>
      <td>FormContext</td>
      <td>在列表项的新增、修改、查看页面时，可以获取当前SPFormContext对象<br/>可以学习一下[Jianyi大神是如何玩这个对象的](http://www.cnblogs.com/jianyi0115/archive/2008/03/12/1102784.html)：<br/>`foreach (BaseFieldControl f in this.ItemContext.FormContext.FieldControlCollection)
            {
                try
                {
                      //some valid code here --if(f.FieldName="XX") do something...

                    if (!f.Field.ReadOnlyField)
                        listItem[f.FieldName] = f.Value;
                }
                catch (ArgumentException) { }
            }`</td>
   </tr>
   
</table>

参考：[SharePoint开发中可能用到的各种Context（上下文）](http://www.cnblogs.com/erucy/archive/2012/08/25/2655600.html)

[WSS页面定制系列（3）---重写表单的保存逻辑](http://www.cnblogs.com/jianyi0115/archive/2008/03/12/1102784.html)