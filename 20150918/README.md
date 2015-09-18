# PowerShell实战 第四回 管理AD计算机对象
    作者：小敏

时间过得真快，又到周末啦，中秋国庆也快到了，想想都有点激动。那么今天来学习点什么呢？就介绍几条比较常用的计算机管理的powershell命令吧。


## 获取计算机对象

`Get-ADComputer –Filter *"`

如果想获取某个OU的计算机对象，可以加searchBase的命令来限制OU，如下：

`Get-ADComputer –Filter * -searchBae "OU=melody,DC=contoso,DC=com" | select Name`

## 查询90天内有登陆域的计算机账号

`Search-ADaccount -AccountInactive -Timespan 90 –ComputersOnly | select Name，LastLogondate`

还有一条命令也可以筛选出，但是不能显示出LastLogondate

`$lastLogon = (get-date).adddays(-90).ToFileTime()`
`Get-ADComputer -filter {lastLogonTimestamp -gt $lastLogon}`

再附上一条更复杂点的命令

`Get-ADComputer -Filter * -Properties * | ?{((get-date) - $_.LastlogonDate).days -gt 365} | Move-ADObject -TargetPath "ou=离职人员计算机账户,dc=contoso,dc=com" –verbose`

## 查询域内计算机系统信息

`Get-ADComputer -Filter * -Property * | Select-Object Name,OperatingSystem,OperatingSystemServicePack,OperatingSystemVersion, LastLogonDate | Export-CSV AllWindows.csv -NoTypeInformation -Encoding UTF`

## 查询计算机的IP信息

`Get-ADComputer -Filter {Name -Like "Computer-Name"} -Properties IPv4Address | Format-List Name,DnsHostName,IPv4Address`

好了，今天的介绍就到这里。下次再见！
