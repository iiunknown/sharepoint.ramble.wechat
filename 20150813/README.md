#如何修改页面左上角的“SharePoint Logo”引发的...
  作者：柒月
  
昨天，在[霖雨](http://www.cnblogs.com/jianyus/)的技术群（72637444）里，一位小伙伴问了这样的问题，“有没有办法把页面左上角的SharePoint字给隐藏掉”，紧接者，一位大神说“当然可以，网上有很多”。我一拍大腿，阿哈，这么简单的问题，终于轮到我出场了，容我度娘一把。。


###关键字
然而，这并没有什么用。度娘有答案，却不知道如何去描述问题，把什么`关键字`丢给搜索引擎。相信很小伙伴特别是初学者，常常遇到这样的囧况。

这个时候，我们往往需要`提炼关键字`——先思考，别无脑。(很押韵，完美。)

在上述问题中，我相信大部分人都能想到：“SharePoint Logo”是个`HTML标签`，在母版页上。那么问题的关键就变成：如何隐藏母版页标签(已知标签的id或者class)->母版页插入自定义js或者css。

更准确一点，既然我要隐藏这个标签，那我就直接以标签id或者class，作为关键字好了。于是，在浏览器中按`F12`（开发者工具），就获得了我的关键字（标签class）：`ms-core-brandingText`

###搜素引擎
很多技术都鄙视度娘，的确有时很挫（度娘几乎搜不到我们辛辛苦苦写在GitHub上的文章 T_T），有时也很好用（度娘已经成为我看[霖雨](http://www.cnblogs.com/jianyus/)大神博客的入口）。

Google很好用，比较细节的一些疑难杂症，Google有时能给出很好的答案（比如拿一些`error code`作为关键字时），可惜需要翻墙。

小敏前段时间向我们推荐的[图灵搜索](https://www.tulingss.com)也非常好用，广告打得也很漂亮：图灵搜索，中国第一个专为程序员打造的搜索引擎。

###知识获取
关键字有了，相信离真相也就不远了。隐藏“SharePoint Logo”至少也有以下几种方法：

- 修改母版页——[How to Change “SharePoint” Text in Suite Bar](http://sharepoint.rackspace.com/sharepoint-2013-how-to-change-sharepoint-branding-text-in-the-upper-left-part-of-your-screen)。个人感觉操作比较复杂，并且不利于环境迁移
- 使用PowerShell——[Change SharePoint 2013 Title](http://www.topsharepoint.com/change-sharepoint-2013-title)。PowerShell很强大，"Shell Everything"，可以学习小敏的[PowerShell系列](https://github.com/iiunknown/sharepoint.ramble.wechat/blob/master/20150708/README.md)
- 使用Module推送DelegateControl——[SP 2013: Some new DelegateControl additions to the SharePoint 2013 master pages](http://zimmergren.net/technical/sp-2013-some-new-delegatecontrol-additions-to-the-sharepoint-2013-master-pages)。可以学习如何使用Module推送，了解DelegateControl的运作方式

###后记
正如大家所看到的，整片文章满满的都是广告，请收益者尽快打5毛到我的账户，谢谢。
