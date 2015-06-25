# Integrating SharePoint with OAM
    作者：杨柳@水杉网络

## OAM 介绍
Oracle Access Management 简称OAM，是Java平台上企业级安全应用程序，在企业应用安全上提供了全方位的服务，包含：Web周边的安全管控，Web单点登录，身份上下文，身份验证与授权，策略管理，测试，日志，审计等等功能。


## 联姻之美

SharePoint作为微软提供的重量级企业协同应用平台，在越来越多的大企业中占据自己特有的位置。对于很多大企业来讲，业务平台不只是单纯的M家，O家，I家，往往集成了各个厂商的优秀企业应用，那么，身份验证与授权及单点登录显得尤为重要。

SharePoint本身就是Web应用，所以用OAM来管理它的身份验证，授权，单点登录等也在情理之中，Oracle官网上有专门针对[OAM与SharePoint集成的帮助文档](http://docs.oracle.com/cd/E40329_01/admin.1112/e27239/oam11_sharepoint2010.htm#AIAAG7317)。原理就是通过OAM专门针对IIS的WebGate进行授权，管理。一切看上去都那么自然，和谐。但毕竟是跨平台跨厂商的两个产品，是水乳交融还是貌合神离？一切依靠实践来证明。

## 实践之路
OAM与SharePoint集成，本身提供了两种模式：

* Windows 模拟验证集成
* 使用LDAP Membership Provider集成

下面介绍一下常见的`Windows 模拟验证集成`在实际项目实践过程中的注意点。

### 软件环境介绍
* 操作系统： Windows Server 2k8 R2 SP1
        注意：官方介绍可以在Windows Server 2012上安装OAM，但在实际安装过程中发现很多不可控问题，遂放弃。
* SharePoint Foundation 2013
        这是坑之一，官方文档上满屏都是介绍SharePoint Server与OAM的集成，并且信誓旦旦说大多数情况需要配置User Profile Service同步人员信息才算集成通过。这让准备使用SharePoint Foundation进行测试的我夜不能寐。但最终证明，这都是纸老虎。
* OAM 安装包: Oracle_Access_Manager10_1_4_3_0_CR2_Win64_ISAPI_WebGate
* SharePoint Web Application基于AD的Windows 认证

### 配置过程介绍
具体的安装配置OAM的过程不在这里累述，请参考官方文档。下面介绍实践过程中的重要注意点：
* WebGate ISAPI Filter只需要在需要使用OAM功能的WebApplication IIS站点上配置即可。
* WebGate HttpModule需要在IIS全局注册，同上，需要在使用OAM功能的WebApplication IIS站点上配置。
*  ***重要：使用OAM功能的WebApplication IIS站点身份认证需要开启ASP.NET 模拟，Form 身份验证，匿名身份验证。***
        这是坑之一：如果不开启Form 身份验证，意思OAM身份验证成功后跳转到SharePoint站点，在IIS验证通过后SharePoint也会抛出经典的``，而官方文档只是说一定要开启ASP.NET 模拟和匿名身份验证。

## 给自己挖坑
OAM与SharePoint集成过程还算顺畅，但是第一次上手的同学或多或少会掉到一些坑里，以上配置目前还是停留在测试环境，在生产环境上正式运行后，我会起长篇介绍集成原理，集成过程，至于是什么时候，您猜？

