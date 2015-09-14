# Get-SPWebApplication &amp; Set-SPWebApplication
	作者：jingnansu

## Get-SPWebApplication     
> - 获取指定的Web应用程序.
> - 参数
>  + Identity: 指定Web应用程序的名称, URL或GUID.
>  + IncludeCentralAdministration: 返回的集合中包含管理中心的Web应用程序.
> - 示例
>  + `Get-SPWebApplication http://sitename`

## Set-SPWebApplication     
> - 设置指定的Web应用程序.
> - 参数
>  + Identity: 指定Web应用程序的名称, URL或GUID.
>  + SMTPServer: 指定Web应用程序将使用的新的出站SMTP服务器.
>  + Zone: 设置区域信息.
>  + AuthenticationMethod: 设置身份验证方式设置为经典Windows身份验证.
>  + AuthenticationProvider: 设置Web应用程序的验证提供程序.
>  + SecureSocketsLayer: 设置SSL加密.
>  + SignInRedirectProvider: 设置登录重定向URL.
>  + SignInRedirectURL: 设置Web应用程序的登录重定向URL.
> - 示例
>  + `Get-SPWebApplication http://somesite | Set-SPWebApplication -OutgoingEmailAddress "user@contoso.com"`

enjoy SharePoint
