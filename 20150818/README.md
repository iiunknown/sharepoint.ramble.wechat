# 检查SharePoint代码中对象是否释放的小工具
	作者：jingnansu

## 问题
一般继承IDisposable接口的对象, 在使用完成后可以释放对象以免占用过多的资源, 当然也可是使用using语句来完成此操作. 但是当项目很大的时候, 代码也比较多, 我们更多希望有工具来检查项目中是否有类似的对象没有释放.

## 解决办法
微软其实提供了一个小工具"SharePoint Dispose Checker Tool", 但是很遗憾, 不支持SharePoint 2013, 有需求的朋友可以点击下面的链接查看.

地址: https://gallery.technet.microsoft.com/office/SharePoint-Dispose-Checker-01da48e8

enjoy SharePoint
