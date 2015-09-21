# Remove-SPWebApplication
	作者：jingnansu

删除指定的Web应用程序.

## 参数     
> - Identity: 指定Web应用程序的名称, URL或GUID.
> - Zone: 从Default, Intranet, Internet, Extranet或者Custom区域中删除一个.
> - DeleteIISSite: 删除关联的IIS网站, 默认不删除.
> - RemoveContentDatabases: 删除关联的内容数据库, 默认不删除.

## 示例
> - `Remove-SPWebApplication http://sitename -Confirm -DeleteIISSite -RemoveContentDatabases`

enjoy SharePoint