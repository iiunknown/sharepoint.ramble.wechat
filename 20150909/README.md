# PowerShell实战 第一回 管理AD组对象
    作者：小敏

从今天起我们开始实战系列了，先从AD讲起，后续会出Exchange和Lync系列，sharepoint系列的可乐哥早就出版了，哈哈。


## 列出某个组的组成员

`Get-ADGroupMember GroupName | Format-Table Name`

## 获取域内的安全-通用组

`Get-ADGroup –LDAPFilter "(&(objectCategory=group)(groupType:1.2.840.113556.1.4.803:=-2147483640))" -Properties * | select Name, DistinguishedName, Description | Export-CSV “安全-通用组.csv” -NoTypeInformation -Encoding UTF8`

## 获取域内的安全-全局组

`Get-ADGroup –LDAPFilter "(&(objectCategory=group)(groupType:1.2.840.113556.1.4.803:=-2147483646))" -Properties * | select Name, DistinguishedName, Description | Export-CSV “安全-全局组.csv” -NoTypeInformation -Encoding UTF8`

## 获取域内的安全-本地域组

`Get-ADGroup –LDAPFilter "(&(objectCategory=group)(groupType:1.2.840.113556.1.4.803:=-2147483643))" -Properties * | select Name, DistinguishedName, Description | Export-CSV “安全-本地域组.csv” -NoTypeInformation -Encoding UTF8`

## 获取组内的成员
`Get-ADGroupMember CSAdministrator | select Name, distinguishedName | Export-CSV CSAdministrator.csv -NoTypeInformation -Encoding UTF8`

## 将A组的用户移动到B组

`Get-ADGroupMember A_group | Select sAMAccountName | ForEach { Add-ADGroupMember B_group -Members $_.sAMAccountName }`

好了，今天的介绍就到这里。下次再见
