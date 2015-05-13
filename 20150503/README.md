# 使用Powershell导出SharePoint列表数据
    作者：杨柳@水杉网络

> 导出列表数据到csv文件，以便借助Excel等工具进行数据分析。SharePoint在UI上提供了列表数据导出到Excel的功能，但是受数据量，超时时间等影响（目前没有找到官方确认说法，继续搜寻中）。使用此脚本可以避免这样的风险，而且还可以扩展代码进行导出前一定业务逻辑的处理。

### 适用版本
- SharePoint 2010
- SharePoint 2013

### 使用方式
1. 下载脚本[list.export2csv.ps1](https://github.com/iiunknown/sharepoint.powershell/blob/master/list.export2csv.ps1)。
2. 参考下载脚本中的注释提示，按照实际需要修改脚本。
3. 在Powershell命令窗口中运行脚本文件。

### 脚本
```powershell
############################################################
#	ShuiShan S2 export list data to csv file.
#	Version: 0.0.0.1
#	12/19/2014
#	http://www.shuishan-tech.com
#	Author: YangLiu@上海水杉网络科技有限公司
#	Description:
#	导出列表数据到csv文件，以便借助Excel等工具进行数据分析。
#	SharePoint在UI上提供了列表数据导出到Excel的功能，但是受数据量，超时时间等影响（目前没有找到官方确认说法，继续搜寻中）。
#	使用此脚本可以避免这样的风险，而且还可以扩展代码进行导出前一定业务逻辑的处理。
############################################################

#判断当前上下文环境中是否装在了SharePoint的Powershell环境，如果没有装载，则装载到当前运行环境。
$Snapin = get-PSSnapin | Where-Object {$_.Name -eq 'Microsoft.SharePoint.Powershell'}
if($Snapin -eq $null){
    Write-Output -InputObject "Loading SharePoint Powershell Snapin"
	try
	{
    	Add-PSSnapin "Microsoft.SharePoint.Powershell"
		Write-Output -InputObject "Loaded SharePoint PowerShell Snapin"
	}
	catch
	{
		$(throw "There was an error loading SharePoint PowerShell Snapin")
		exit
	}
}

#获取Web对象, 根据需要替换为自己的网站地址。
$web = Get-SPWeb http://demo.shuishan-tech.com
#获取List对象，根据需要替换为自己的列表名称。
$list = $web.Lists["SSO_User"]
#获取列表所有数据。
$list.GetItems() |
#在管道中循环获取到得列表数据。
ForEach-Object{
	#构建psobject。
    $obj = New-Object psobject;
    #把每一行SPListItem上的数据注入到构建的psobject对象实例中，如果是SPListItem内建字段，可以使用 $_.ID 这样的格式获取。
    Add-Member -MemberType noteproperty -InputObject $obj -Name "ID" -Value $_.ID;
    Add-Member -MemberType noteproperty -InputObject $obj -Name "Title" -Value $_.Title;
    Add-Member -MemberType noteproperty -InputObject $obj -Name "Created" -Value $_.Craeted;
    Add-Member -MemberType noteproperty -InputObject $obj -Name "Modified" -Value $_.Modified;
    #也可以获取自定义字段，需要使用 $_["FieldName"] 这种方式获取。
    Add-Member -MemberType noteproperty -InputObject $obj -Name "CustomField" -Value $_["CustomField"];
    ################
    #提示：可以在这里注入自己的业务逻辑，对SPListItem获取到得数据进行格式化，然后注入到对象中。
    ################

    #输出包含数据的对象。
    $obj
} |
#在下一个管道中输出对象中的数据到csv文件，如果包含中文字符，需要使用 -Encoding UTF8的属性。
Export-Csv c:\list.csv -Encoding UTF8
```
