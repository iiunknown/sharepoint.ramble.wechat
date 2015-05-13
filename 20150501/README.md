# How To Wake Up SharePoint
    作者：sujingjiang

我们都知道SharePoint是基于ASP.NET的, ASP.NET有个特点就是第一个用户第一次访问的时候会进行JIT编译, 这样就造成了页面打开比较慢; 另外由于托管网站的应用程序池会进行回收, 回收后再次访问又会发生编译动作, 所以这就是为啥之前访问比较快, 忽然第二天后又慢了.

现在大家知道了问题所在, 那么解决问题的方法就很简单了; 我们在第一个用户访问之前, 通过某种方法让JIT编译提前进行, 那么当用户在真正访问的时候页面打开就会比较快了.

我们可以参考"SharePoint 2007, 2010 or 2013 Application Warm-up Script", 将一段PS脚本设置成每天定时自动运行; 运行后我们再去访问网站集, 这时候页面就会很快的打开了. 另外Codeplex上也有一个小工具大家也可以参考"SPWakeUp - Wake up your SharePoint and WSS Sites".

当然这种方式是从外部入手, 从内对SharePoint网站优化是有很多方面的, 我们将会在以后为大家慢慢介绍.

enjoy SharePoint
