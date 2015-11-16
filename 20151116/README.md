#唤醒你的网站——SharePoint WarmUp
	作者：柒月
每天第一次访问SharePoint网站，它的表现总是很反常。页面加载时间由平时的几秒，变成十几秒甚至几十秒，紧接着又恢复了正常。那么，我们在睡觉的时候，SharePoint究竟在做什么？

###SharePoint也需要休息
服务了一天，无数的页面被打开和关闭，无数的对象被创建和销毁，自然产生了很多的内存碎片和磁盘碎片。为了提高响应速度和执行效率，是时候来一记“一键清理”了。哦，在安卓手机上才叫“一键清理”，在SharePoint（IIS网站）上，那叫做回收应用程序池。 :)

回收完应用程序池，SharePoint脑袋一片空白。直到有用户访问它，才想起来，还需要JIT编译！JIT编译花费了不少时间，这就是为什么第一次访问SharePoint时，它像是睡着了一般。
###嘿，别睡了，起来干活
既然回收应用程序池和JIT编译无法避免，且时间不可知（计划时间，或者内存达到阈值时进行回收），于是大神们想了一个方法，定时“叫醒”它——SharePoint WarmUp。WarmUp的思路是，以模拟用户的方式，每隔一段时间，访问SharePoint网站，这样，在第一个真实用户访问前，就完成了JIT编译。之前已经介绍过[TimerJob的调度](https://github.com/iiunknown/sharepoint.ramble.wechat/blob/master/20150701/README.md)和[使用WebClient读取SharePoint文件](https://github.com/iiunknown/sharepoint.ramble.wechat/blob/master/20150716/README.md)，两者结合起来，稍加修改就可以完成此功能啦。

如何写一个定时任务，请继续强势围观[霖雨大神的Blog](http://www.cnblogs.com/jianyus/p/3458535.html)。当然，Windows计划任务调用PowerShell也可以胜任这一工作。

脚本详见尾部参考部分。

###思考
- Windows计划任务与SharePoint Timer的取舍？Windows计划任务可以一键安装，可以随时更改脚本逻辑；Timer部署后更改代码逻辑较困难，但在CA中管理，用户体验较为统一。
- 多台前端做负载均衡，可以通过`（PowerShell）Get-SPAlternateUrl`和`（C#）SPWebApplication.AlternateUrls`，从AAM中获取各前端服务器的访问地址，逐个WarmUp。之前我们也介绍过[SharePoint备用访问映射](https://github.com/iiunknown/sharepoint.ramble.wechat/blob/master/20150730/README.md)。
- 将WarmUp脚本加入到[自动化部署SharePoint解决方案](https://github.com/iiunknown/sharepoint.ramble.wechat/blob/master/20150505/README.md)，也许能省去与客户解释“为什么慢慢慢”的时间。

参考：

[Configuring Recycling Settings for an Application Pool (IIS 7)](https://technet.microsoft.com/en-us/library/cc753179(WS.10).aspx)

[如何对SharePoint网站进行预热(warmup)以提高响应速度](http://www.cnblogs.com/chenxizhang/p/3271990.html)

[Thoughts on SharePoint Application Pools, Recycling and "JIT Lag"](http://weblogs.asp.net/erobillard/thoughts-on-sharepoint-application-pools-recycling-and-quot-jit-lag-quot)

[SPBestWarmUp](https://spbestwarmup.codeplex.com/)

[SharePoint 2013 图文开发系列之计时器任务](http://www.cnblogs.com/jianyus/p/3458535.html)
