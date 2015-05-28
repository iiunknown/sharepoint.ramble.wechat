# 操作WSP解决方案包的相关命令
	作者：sujingjiang

## Add-SPSolution
> - 例子:```Add-SPSolution -LiteralPath c:\solution.wsp
```
> - 注意该命令只是向SharePoint服务器场添加解决方案包, 并不会做其他操作.

## Install-SPSolution
> - 例子: ```Install-SPSolution -Identity solution.wsp -GACDeployment
```
> - 参数:
>  + "-Force" 强制部署新的解决方案包.
>  + "-AllWebApplications" 向服务器场中所有Web应用程序部署新的解决方案包.
>  + "-FullTrustBinDeployment" 允许完全信任Bin部署.
>  + "-GACDeployment" 部署到全局程序集缓存.
>  + "-Time" 在指定的时间部署解决方案包, 默认是立刻部署.
>  + "-WebApplication" 为指定的Web应用程序部署新的解决方案包.

## Update-SPSolution
> - 例子: ```Update-SPSolution -Identity solution.wsp -LiteralPath c:\solutionv2.wsp -GACDeployment
```
> - 参数:
>  + "-Force" 强制更新解决方案包.
>  + "-GACDeployment" 部署到全局程序集缓存.
>  + "-Time" 在指定的时间更新解决方案包, 默认是立刻部署.

## Uninstall-SPSolution
> - 例子: ```Uninstall-SPSolution -Identity solution.wsp
```
> - 注意该命令只是收回卸载已部署的解决方案包, 并不会做其他操作.
> - 参数:
>  + "-AllWebApplications" 从服务器场中所有Web应用程序回收卸载解决方案包.
>  + "-WebApplication" 从指定的Web应用程序回收卸载解决方案包.
>  + "-Time" 在指定的时间回收卸载解决方案包, 默认是立刻回收卸载.

## Remove-SPSolution
> - 例子: ```Remove-SPSolution -Identity solution.wsp
```
> - 参数:
>  + "-Force" 强制删除解决方案包.

enjoy SharePoint
