#使用MSBuild打包SharePoint项目
	作者：柒月
[MSBuild](https://msdn.microsoft.com/en-us/library/ms171452(v=vs.90).aspx)(Microsoft Build Engine)是 Microsoft 和 Visual Studio 的生成平台，它可以帮助开发人员在未安装 Visual Studio 的环境中组织和生成项目，我们也可以使用MSBuid将SharePoint项目打包成wsp。

##步骤

1、打开cmd，并定位到MSBuild.exe所在目录(我的环境是C:\Windows\Microsoft.NET\Framework64\v4.0.30319\)

2、打入以下命令，那么在默认(项目所在\bin目录下)路径下将生成一个wsp包。此处`ProjectFilePath`是SharePoint项目的路径，例如C:\Solution1\Pro1\Pro1.csproj

```
msbuild /t:Package "ProjectFilePath"
```
##拓展
以上示例仅是展示MSBuild最基本的用法。在使用MSBuild的基础上，配合源代码管理和强大的PowerShell，我们可以将发布的工作自动化、脚本化，极大地减少错误几率，提高生活质量 :)。

推荐文章:

[MSBuild入门](http://www.cnblogs.com/linianhui/archive/2012/08/30/2662648.html)

[基于 Jenkins 快速搭建持续集成环境](http://www.cnblogs.com/shanyou/p/3750714.html)
