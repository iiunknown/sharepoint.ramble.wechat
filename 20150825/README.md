# New-SPWebApplication
	作者：jingnansu

创建新的 Web 应用程序.

## 特殊参数
> - "AllowAnonymousAccess": 允许进行匿名访问.
> - "ApplicationPoolAccount": 将运行此应用程序池的用户帐户的身份.
> - "AuthenticationMethod": 使用 Kerberos 或 NTLM 身份验证方法, 默认是 NTLM.
> - "DatabaseCredentials": 指定数据库用户帐户
> - "Path": 指定新的 Web 应用程序的物理目录.

## 例子
> - `New-SPWebApplication -Name "Site" -Port 80 -HostHeader sharepoint.contoso.com -URL "https://www.contoso.com" -ApplicationPool "ContosoAppPool" -ApplicationPoolAccount (Get-SPManagedAccount "DOMAIN\sharepointinstall") -AuthenticationProvider $ap -SecureSocketsLayer`

enjoy SharePoint
