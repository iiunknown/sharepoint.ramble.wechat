# Get-SPSite &amp; Set-SPSite
	作者：sujingjiang

这两个命令一般情况下是一起出现的, 一个是获取指定条件的网站集, 另一个是设置对应的网站集参数.

## 例子     
> - 获取指定的内容数据库中所有网站集地址和主要管理员
>  + `Get-SPSite -ContentDatabase "WSS_Content_7f74cfcb9d484b91802e973c5361139a" | Format-Table -Property Url, Owner`
>  + ![](imgs/20150612.1.png)
> - 获取指定路径的网站集中的子网站的标题和URL
>  + `Get-SPSite 'http://sitename' | Get-SPWeb -Limit All | Select Title, URL`
>  + ![](imgs/20150612.2.png)
> - 设置指定网站集的第二管理员
>  + `Get-SPSite http://sitename | Set-SPSite -SecondaryOwnerAlias "DOMAIN\username"`
> - 设置指定网站集只读
>  + `Get-SPSite http://sitename | Set-SPSite -LockState ReadOnly`
> - 设置指定网站集的配额
>  + `Set-SPSite -identity "http://sitename" -MaxSize 1000000 -WarningSize 500000`

enjoy SharePoint
