#使用WebClient读取SharePoint文件
在某些情境下，我们可能需要外部系统访问SharePoint文档库中的文件。在拥有文件完整Url的情况下，我们可以通过WebClient去访问文件。
###访问
与SP客户端对象模型一样，若与SharePoint不在同一域下，需要先解决认证问题：
``` C#
string url="http://spserver/DocLib/Pic1.png"; //DocLib是文档库名；Pic1.png是文件名
WebClient client = new WebClient();
client.Credentials = new NetworkCredential("UserName","Password","Domain"); //AD环境 模拟登陆

//ClientContext context = new ClientContext("http://spserver"); //SharePoint客户端对象模型
//context.Credentials = new NetworkCredential("UserName","Password","Domain"); 

byte[] data = client.DownloadData(url);//下载文件
```
###输出
若需要将图片输出到页面上，供其他`<img/>`引用，则使用如下代码：
```C#
Response.ContentType = "image/jpeg";
Response.BinaryWrite(data);
Response.End();
```

若需要下载，则使用如下代码：
``` C#           
Response.ContentType = "application/octet-stream";
//通知浏览器下载文件而不是打开
Response.AddHeader("Content-Disposition", "attachment; filename="+FileName);
Response.BinaryWrite(data);
Response.Flush();
Response.End();
```
