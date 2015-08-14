# 《SharePoint 漫谈》微信订阅号内容管理

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/iiunknown/sharepoint.ramble.wechat?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

![微信订阅号二维码](sharepoint.ramble.wechat.jpg)

《SharePoint 漫谈》微信订阅号是由一群兴趣相投的SharePoint从业小伙伴儿共同发起的SharePoint相关资讯分享的信息通道，于2015年4月21日开始运营。

我们关注SharePoint相关资讯，也关注以SharePoint为基点辐射开的行业资讯，如IT基础架构建设，运维，互联网技术资讯等。

我们希望利用SharePoint这个超过15年的企业Web应用平台在当下排山倒海般的互联网应用，移动应用中找到自我技术的立足点。

我们希望以一通百通的崇高目标作为自我学习的推动力，进而反哺影响企业应用的实现。

我们希望以此微信订阅号为平台，进行SharePoint开发技术，行业资讯的传播与交流。


##贡献者
* [iiunknown](https://github.com/iiunknown)
* [jingnansu](https://github.com/jingnansu)
* [Melody](https://github.com/melodytu)
* [lostcowteng](https://github.com/lostcowteng)

如果您愿意加入我们，请提供一篇内容发起Pull request，我们会定期审核。

## 撰写标准
###工具依赖
* 使用[GitBook](https://www.gitbook.com)方式组织内容，定期发布到GitBook，形成可下载的电子书。
* 使用[Markdown](https://github.com/riku/Markdown-Syntax-CN)格式书写。
* 推荐Github客户端管理内容同步。
* 推荐使用Gitbook客户端进行内容章节管理。
* Windows下推荐使用[Markdownpad](http://www.markdownpad.com/)进行内容撰写。
* Mac下推荐使用[Macdown](https://github.com/uranusjr/macdown)进行内容撰写。

### 内容撰写及提交过程
1. 先Fork本项目到自己的Github账号下。
2. 创建以自己Github账号为名称的文件夹，我们称之为`个人文件夹`。
3. 在`个人文件夹`下使用文件夹管理每篇内容，每篇内容的文件夹我们称之为`内容文件夹`。
4. `内容文件夹`下必须包含一个`README.md`的内容文件，文章内容就在其中。
5. 如果文章包含图片，那么在`内容文件夹`下创建名为`imgs`的文件夹，文章内容使用的图片全部存放于此。
6. 完成文章内容后，发起`Pull Request`到`prepublishing`分支，请求新内容整合到我们的内容管理库中。
7. 审核通过后，我们会择时在微信订阅号推送内容。
8. 发布内容后，管理员会在本项目下创建以日期格式yyyyMMdd为名称的目录，复制文章内容，创建Gitbook对应章节，发布到Gitbook供生成电子书。
![个人文件夹内容组织示例](content.png)

### 内容格式
1. 必须包含以下内容：
    * 以`# 题目`Markdown格式开始的标题
    * 以`## 摘要`Markdown格式的内容摘要描述
2. 建议
    * 以`## 章节题目`Markdown格式分隔内容区块，方便微信订阅号发布，方便读者阅读。
    * 如果有`阅读原文`链接，在内容最后以`> `Markdown引用格式包含原文地址。


## 已发布内容索引

* [20150421 在爱恨之间](20150421/README.md)
* [20150423 HTTP协议学习资料推荐](20150423/README.md)
* [20150424 如何根据Correlation ID来查找具体的错误信息](20150424/README.md)
* [20150425 如何自动登陆 SharePoint 系统?](20150425/README.md)
* [20150426 如何跨网段配置DHCP故障转移](20150426/README.md)
* [20150427 无法使用资源管理器浏览文档库?](20150427/README.md)
* [20150429 浅析SPField的DisplayName，InternalName，StaticName](20150429/README.md)
* [20150430 SharePoint 2013 子网站继承父网站树视图方法](20150430/README.md)
* [20150501 How To Wake Up SharePoint](20150501/README.md)
* [20150502 Build 2015之：从Azure到SharePoint](20150502/README.md)
* [20150503 使用Powershell导出SharePoint列表数据](20150503/README.md)
* [20150504 怎么在 InfoPath Web 浏览器表单中弹出对话框?](20150504/README.md)
* [20150505 使用Powershell自动化部署SharePoint解决方案](20150505/README.md)
* [20150506 使用图形化界面部署WSP解决方案包](20150506/README.md)
* [20150507 SharePoint 2013安装之：AppFabric安装错误解决办法](20150507/README.md)
* [20150508 使用AutoSPInstaller自动安装部署SharePoint](20150508/README.md)
* [20150511 使用Powershell更改SharePoint欢迎页面](20150509/README.md)
* [20150512 “新”那么点事](20150512/README.md)
* [20150513 如何修改SharePoint服务器数据库连接字符串](20150513/README.md)
* [20150514 使用HTTPS协议集成SharePoint 2013和OWA](20150514/README.md)
* [20150515 变，便](20150515/README.md)
* [20150519 如何修改管理中心端口号](20150519/README.md)
* [20150520 SharePoint技术聚会（2015年5月23日）](20150520/README.md)
* [20150521 SPHttpUtility.HtmlEncode 介绍](20150521/README.md)
* [20150522 SharePoint PowerShell命令系列 (1) Backup-SPSite & Restore-SPSite](20150522/README.md)
* [20150525 SPHttpUtility.HtmlDecode 介绍](20150525/README.md)
* [20150526 SharePoint PowerShell命令系列 (2) Export-SPWeb & Import-SPWeb](20150526/README.md)
* [20150527 文档库的版本控制——SPFileVersion](20150527/README.md)
* [20150528 SharePoint PowerShell命令系列 (3) 操作WSP解决方案包的相关命令](20150528/README.md)
* [20150529 Ajax.NET for SharePoint](20150529/README.md)
* [20150601 SPUtility.CreateISO8601DateTimeFromSystemDateTime](20150601/README.md)
* [20150602 SharePoint迁移之配置数据库连接](20150602/README.md)
* [20150603 SharePoint PowerShell命令系列 (4) Get-SPSolution](20150603/README.md)
* [20150604 SharePoint的门票——基于声明的身份验证](20150604/README.md)
* [20150605 使用Schema定义推送列表模板及实例注意事项](20150605/README.md)
* [20150608 灵活运用VS外部工具：获取程序集全名称](20150608/README.md)
* [20150609 SharePoint PowerShell命令系列 (5) New-SPSite](20150609/README.md)
* [20150610 无码制作一个InfoPath LOV](20150610/README.md)
* [20150611 灵活运用VS外部工具：Copy assembly to GAC](20150611/README.md)
* [20150612 SharePoint PowerShell命令系列 (6) Get-SPSite &amp; Set-SPSite](20150612/README.md)
* [20150615 SharePoint Manager 2013](20150615/README.md)
* [20150616 SharePoint PowerShell命令系列 (7) Move-SPSite](20150616/README.md)
* [20150617 页面访问授权的秘密：SPUtility.EnsureAuthentication](20150617/README.md)
* [20150618 获取登录用户显示名：SPUtility.GetDisplayUserName](20150618/README.md)
* [20150619 捞数据请节制——SPList阈值](20150619/README.md)
* [20150623 SharePoint配套产品概览](20150623/README.md)
* [20150624 LINQ to SharePoint](20150624/README.md)
* [20150625 SharePoint Feature Tool](20150625/README.md)
* [20150626 Integrating SharePoint with OAM](20150626/README.md)
* [20150629 合并Url：SPUtility.ConcatUrls](20150629/README.md)
* [20150630 SharePoint PowerShell命令系列 (8) Remove-SPSite](20150630/README.md)
* [20150701 TimerJob的调度](20150701/README.md)
* [20150702 格式化日期时间：SPUtility.FormatDate](20150702/README.md)
* [20150703 友好显示字节长度：SPUtility.FormatSize](20150703/README.md)
* [20150706 Active Directory Tools For SharePoint](20150706/README.md)
* [20150707 获取目录在SharePoint安装路径下的全路径：SPUtility.GetGenericSetupPath](20150707/README.md)
* [20150708 PowerShell介绍 第一回 "Shell Everything"](20150708/README.md)
* [20150709 几个开源的CAML生成类](20150709/README.md)
* [20150710 PowerShell介绍 第二回 "PowerShell执行策略"](20150710/README.md)
* [20150713 浅析场解决方案部署位置](20150713/README.md)
* [20150714 Google Analytics For SharePoint](20150714/README.md)
* [20150715 PowerShell介绍 第三回 "PowerShell导入导出"](20150715/README.md)
* [20150716 使用WebClient读取SharePoint文件](20150716/README.md)
* [20150717 判断传出电子邮件是否配置：SPUtility.IsEmailServerSet](20150717/README.md)
* [20150720 PowerShell介绍 第四回 比较符、逻辑符和运算符](20150720/README.md)
* [20150721 SharePoint PowerShell命令系列 (9) New-SPWeb](20150721/README.md)
* [20150722 SPField.InternalName命名规则探索](20150722/README.md)
* [20150724 使用MSBuild打包SharePoint项目](20150724/README.md)
* [20150727 获取网站数据库服务器当前时间：SPUtility.GetServerNow](20150727/README.md)
* [20150728 SharePoint PowerShell命令系列 (10) Get-SPWeb & Set-SPWeb](20150728/README.md)
* [20150729 PowerShell介绍 第六回 WMI介绍](20150729/README.md)
* [20150730 SharePoint备用访问映射](20150730/README.md)
* [20150731 判断是否合法文件名：SPUrlUtility.IsLegalFileName](20150730/README.md)
* [20150803 判断是否是全路径Url：SPUrlUtility.IsUrlFull](20150803/README.md)
* [20150804 SharePoint PowerShell命令系列 (11) Remove-SPWeb](20150804/README.md)
* [20150805 PowerShell介绍 第七回 变量](20150805/README.md)
* [20150806 VS部署SharePoint解决方案](20150806/README.md)
* [20150811 使用VS附加到进程的时候如何选择正确的W3WP进程?](20150811/README.md)
* [20150812 设置当前访问线程的语言：SPUtility.SetThreadCulture](20150812/README.md)
* [20150813 如何修改页面左上角的“SharePoint Logo”引发的...](20150813/README.md)
* [20150813 PowerShell介绍 第八回 数组](20150814/README.md)
