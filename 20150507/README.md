# SharePoint 2013安装之：AppFabric安装错误解决办法
    作者：小敏

安装SharePoint 2013之前必须安装必备组件。必备组件可以分为两种安装方式：
- 在线安装：安装服务器接入到互联网。
- 离线安装：下载必备组件，手动安装。

在使用以上两种安装方式过程中，经常会遇到Windows Server AppFabric组件安装失败，下面介绍常见的安装错误原因及解决方法。

### 在线安装方式
在线安装方式出现错误的最大原因是网络不稳定，因此，建议在安装过程中打开一个长ping外网的命令行窗口，监控网络是否超时，是否掉包，如果有类似情况导致安装失败，则需要解决网络稳定性，或者使用离线安装方式。

### 离线安装方式
离线安装第一步需要准备好所有组件安装包（下载链接请点击文章最后的“阅读原文”）。安装Windows Server AppFabric使用命令行工具（注意，是命令行工具，不是Powershell，使用Powershell窗口执行会报语法错误），进入到安装包所在的目录，使用如下命令：

```cmd
WindowsServerAppFabricSetup_x64.exe" /i CacheClient,CachingService,CacheAdmin /gac
```

祝您安装顺畅:-)

### SharePoint 2013必备组件下载地址

 1. Microsoft .NET Framework 4.5
http://go.microsoft.com/fwlink/?LinkId=225702
http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe
 2. Windows Management Framework 3.0
http://www.microsoft.com/en-us/download/details.aspx?id=34595
 3. Microsoft SQL Server 2008 R2 SP1 Native Client
http://www.microsoft.com/zh-cn/download/details.aspx?id=26728
 4. Windows Identity Foundation (KB974405)
http://go.microsoft.com/fwlink/p/?LinkID=226830
http://download.microsoft.com/download/D/7/2/D72FD747-69B6-40B7-875B-C2B40A6B2BDD/Windows6.1-KB974405-x64.msu
 5. Microsoft Sync Framework Runtime v1.0 SP1 (x64)
http://go.microsoft.com/fwlink/p/?LinkID=224449
http://download.microsoft.com/download/E/0/0/E0060D8F-2354-4871-9596-DC78538799CC/Synchronization.msi
 6. Windows Server AppFabric
http://go.microsoft.com/fwlink/?LinkId=235496
http://download.microsoft.com/download/A/6/7/A678AB47-496B-4907-B3D4-0A2D280A13C0/WindowsServerAppFabricSetup_x64.exe
 7. Windows Identity Extensions
http://go.microsoft.com/fwlink/?LinkID=252368
http://download.microsoft.com/download/0/1/D/01D06854-CA0C-46F1-ADBA-EBF86010DCC6/rtm/MicrosoftIdentityExtensions-64.msi
 8. Microsoft Information Protection and Control Client
http://go.microsoft.com/fwlink/p/?LinkID=219568
http://download.microsoft.com/download/9/1/D/91DA8796-BE1D-46AF-8489-663AB7811517/setup_msipc_x64.msi
 9. Microsoft WCF Data Services 5.0
http://www.microsoft.com/zh-cn/download/details.aspx?id=29306
 10. Cumulative Update Package 1 for Microsoft AppFabric 1.1 for Windows Server (KB2671763)
http://www.microsoft.com/zh-cn/download/details.aspx?id=29241

