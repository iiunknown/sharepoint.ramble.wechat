# 《SharePoint 漫谈》微信订阅号内容管理



![微信订阅号二维码](sharepoint.ramble.wechat.jpg)

《SharePoint 漫谈》微信订阅号是由一群兴趣相投的SharePoint从业小伙伴儿共同发起的SharePoint相关资讯分享的信息通道，于2015年4月21日开始运营。

我们关注SharePoint相关资讯，也关注以SharePoint为基点辐射开的行业资讯，如IT基础架构建设，运维，互联网技术资讯等。

我们希望利用SharePoint这个超过15年的企业Web应用平台在当下排山倒海般的互联网应用，移动应用中找到自我技术的立足点。

我们希望以一通百通的崇高目标作为自我学习的推动力，进而反哺影响企业应用的实现。

我们希望以此微信订阅号为平台，进行SharePoint开发技术，行业资讯的传播与交流。


##贡献者
* [iiunknown](https://github.com/iiunknown)
* [sujingjiang](https://github.com/sujingjiang)
* [Melody](https://github.com/melodytu)

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

* [20150421 在爱恨之间](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206285277&idx=1&sn=26d649bd9098bb6385ee56e4a6ecc9f0#rd)
* [20150423 HTTP协议学习资料推荐](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206329665&idx=1&sn=36059067e6f826ea25b465ed2beadf81#rd)
* [20150424 如何根据Correlation ID来查找具体的错误信息](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206370990&idx=1&sn=a25119e3b8289e02f390d0a502b904b0#rd)
* [20150425 如何自动登陆 SharePoint 系统?](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206403301&idx=1&sn=1ebe1ea6b2ee73358ab576112f5ebad7#rd)
* [20150426 如何跨网段配置DHCP故障转移](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206441707&idx=1&sn=af6a7a097c8230cd24b686b3aa55d35d#rd)
* [20150427 无法使用资源管理器浏览文档库?](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206524025&idx=1&sn=87d7979af0d18250e49aedc9f81c335e#rd)
* [20150428 浅析SPField的DisplayName，InternalName，StaticName（上篇）](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206648018&idx=1&sn=bd6fa8583637d2af4a94966042c513d4#rd)
* [20150429 浅析SPField的DisplayName，InternalName，StaticName（下篇）](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206666057&idx=1&sn=e6f6aef825c126f6f956f61921de6754#rd)
* [20150430 SharePoint 2013 子网站继承父网站树视图方法](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206817761&idx=1&sn=99ae220da5f034131ff8325e21e457be#rd)
* [20150501 How To Wake Up SharePoint](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206856985&idx=1&sn=6fd22d641ea00468eb52b8fceda845d0#rd)
* [20150502 Build 2015之：从Azure到SharePoint](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206860291&idx=1&sn=1cfe1a636221e730976ab4f73c24f481#rd)
* [20150503 使用Powershell导出SharePoint列表数据](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206914604&idx=1&sn=c7f9b1136423af22ca3087a6b500385e#rd)
* [20150504 怎么在 InfoPath Web 浏览器表单中弹出对话框?](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206932293&idx=1&sn=f103c16fb679fafe0ea3a3e7cd5f1862#rd)
* [20150505 使用Powershell自动化部署SharePoint解决方案](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206990261&idx=1&sn=76aa63cea7a9c23f1f11c115a63d3434#rd)
* [20150506 使用图形化界面部署WSP解决方案包](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=206979190&idx=1&sn=95c51a131d1a45c96529da16a457f81a#rd)
* [20150507 SharePoint 2013安装之：AppFabric安装错误解决办法](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=207191646&idx=1&sn=127249a332c36caeea0046694ba85607#rd)
* [20150508 使用AutoSPInstaller自动安装部署SharePoint](http://mp.weixin.qq.com/s?__biz=MzA5ODc0ODY5MQ==&mid=207266100&idx=1&sn=456af25bd8e5e5119892dd820fde3263#rd)

