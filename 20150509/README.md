# 使用Powershell更改SharePoint欢迎页面
    作者：杨柳@水杉网络

## 描述
SharePoint有自己的默认欢迎页，也就是在访问一个SharePoint网站地址时，SharePoint会默认装载的页面，比如访问 [http://demo.shuishan-tech.com](http://demo.shuishan-tech.com)，默认情况下会跳转到[http://demo.shuishan-tech.com/SitePages/主页.aspx](http://demo.shuishan-tech.com) ，在实际使用中，我们需要把欢迎页面定向到我们自己定制好的首页。

在SharePoint Server版本中，如果开启了发布功能，在`网站管理`界面中会提供一个修改欢迎页面的链接。但是如果如果没有开启发布功能或者使用的是SharePoint Foundation版本，那么在`网站管理`中就没有相应的用户界面可供使用。

## 方法
针对以上情况，最简单的办法就是使用Powershell脚本。

```
$web = Get-SPWeb http://demo.shuishan-tech.com;
$folder = $web.RootFolder;
$folder.WelcomePage = "SitePages/home.aspx";
$folder.Update();
```

## 注意事项
一定要使用变量获取`SPWeb`的`RootFolder`，然后再去为`WelcomePage`赋值，更新`RootFolder`。如果直接针对`SPWeb`对象实例的`RootFolder`属性进行操作，则不会产生效果。

<font style="color:red;">***注意，以下代码是无效代码：***</font>
```
$web = Get-SPWeb http://demo.shuishan-tech.com;
#以下代码是错误示例。
#$web.RootFolder.WelcomePage = "SitePages/home.aspx";
#$web.RootFolder.Update();
```
