# Backup-SPSite &amp; Restore-SPSite
这两条命令可能是大家最先接触的PowerShell命令了吧, 一个是备份网站集, 另一个是还原网站集.

## Backup-SPSite
> - 例子: `Backup-SPSite http://site_name -Path C:\Backup\site_name.bak`
> - 参数:
>  + "-Force" 覆盖现有备份.
>  + "-NoSiteLock" 设置备份的时候网站集是否可以读写. 如果没有指定该参数, 那么在备份的过程中网站集将会被设置成只读, 备份完成后网站集恢复原始状态.
>
## Restore-SPSite
> - 例子: `Restore-SPSite http://site_name -Path C:\Backup\site_name.bak`
> - 参数:
>  + "-Force" 覆盖现有URL地址上的网站集
>  + "-ContentDatabase" 指定还原到哪个内容数据库上; 如果没有指定将使用未使用的网站集容量最大, 其数据库状态为准备就绪的内容数据库.
>  + "-DatabaseServer -DatabaseName" 指定数据服务器名称和数据名称