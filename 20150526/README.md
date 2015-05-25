# Export-SPWeb &amp; Import-SPWeb
上期我们介绍了如何备份还原网站集, 可是有时候我们只需要把某个网站或者子站点中的内容导出, 那么就会用到这两条命令.

## Export-SPWeb
> - 例子: `Export-SPWeb http://web_name -Path web.cmp`
> - 参数:
>  + "-Force" 覆盖现有的导出文件.
>  + "-CompressionSize" 设置导出文件的最大大小, 如果超过就会拆成多个文件.
>  + "-IncludeVersions" 设置导出文件中包含的文件和列表项版本历史记录的类型, 如果没有指定则是当前主版本.

## Import-SPWeb
> - 例子: `Import-SPWeb http://web_name -Path web.cmp`
> - 参数:
>  + "-Force" 覆盖现有站点.
>  + "-ActivateSolutions" 设置导入站点的时候是否激活用户解决方案.
>  + "-UpdateVersions" 设置导入到网站的文件版本已经存在于该网站上的时候该如何处理, 默认是增加新版本.

enjoy SharePoint