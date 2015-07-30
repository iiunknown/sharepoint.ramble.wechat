# Get-SPWeb &amp; Set-SPWeb
	作者：jingnansu

## Get-SPWeb     
> - 获取指定的子网站.
> - 参数
>  + Identity: 指定子网站的地址.
>  + Limit: 限制要返回的子网站的最大数量, 默认值为200; 若要返回所有网站, 请输入all.
>  + Site: 指定获取子网站的网站集的地址或GUID.
> - 示例
>  + `Get-SPWeb -site http://sitename/sites/site1`

## Set-SPWeb     
> - 设置指定的子网站.
> - 参数
>  + Identity: 需要设置的子网站地址或者对象.
>  + Name: 设置子网站的名称.
>  + Description: 设置子网站的描述.
>  + Theme: 设置子网站的主题.
> - 示例
>  + `Get-SPWeb http://sitename/subweb | Set-SPWeb -Title "My Site Title"`

enjoy SharePoint
