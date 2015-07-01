#TimerJob的调度
	作者:柒月
如何开发SharePoint TimerJob，请参考[霖雨大神的博客](http://www.cnblogs.com/jianyus/)——[SharePoint 2013 图文开发系列之计时器任务](http://www.cnblogs.com/jianyus/p/3458535.html)。

根据企业的实际业务，我们可能会搭建多服务器架构的SPFarm；或者做SharePoint容量规划时，考虑1个Web Application下建立多个内容数据库的方式。那么，在这种情况下，SharePoint TimerJob是如何调度的呢？

##SPJobDefinition
 关键在于[SPJobDefinition](https://msdn.microsoft.com/en-us/library/ms427704.aspx)（开发TimerJob须要新建类并继承SPJobDefinition）构造函数的两个参数：[SPServer](https://msdn.microsoft.com/en-us/library/microsoft.sharepoint.administration.spserver.aspx)和[SPJobLockType](https://msdn.microsoft.com/en-us/library/microsoft.sharepoint.administration.spjoblocktype.aspx)。

**SPServer**   将该TimerJob安装到指定的服务器上。若传NULL值，则会将TimerJob安装到SPFarm的所有服务器上

**SPJobLockType**   枚举类型

- `SPJobLockType.None`   TimerJob将会运行在所有安装此TimerJob的服务器上：若安装在n台服务器上，系统每次调用[SPJobDefinition.Execute](https://msdn.microsoft.com/en-us/library/microsoft.sharepoint.administration.spjobdefinition.execute.aspx)，则此TimerJob会执行n次


- `SPJobLockType.ContentDatabase` 将为Web Application里的每个内容数据库执行一次：若当前Web Application有n个内容数据库，系统每次调用SPJobDefinition.Execute，则此TimerJob会执行n次



- `SPJobLockType.Job`   不管此TimerJob安装在几台服务器上，系统每次调用SPJobDefinition.Execute，此TimerJob只会执行1次

参考文档：[SharePoint Timer Job Locks and Scope](http://blogs.msdn.com/b/besidethepoint/archive/2011/11/13/sharepoint-timer-job-locks-and-scope.aspx)