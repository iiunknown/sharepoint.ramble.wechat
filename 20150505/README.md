# 使用Powershell自动化部署SharePoint解决方案
    作者：杨柳@水杉网络

### 适用版本
- SharePoint 2010
- SharePoint 2013

### 使用方式
1. 下载脚本[solution.deployment.ps1](https://github.com/iiunknown/sharepoint.powershell/blob/master/solution.deployment.ps1)。
2. 复制`solution.deployment.ps1`脚本到需要部署的解决方案包所在的目录。
3. 修改脚本中第121行中的`$solutionNames`变量值，多个解决方案包使用`分号(;)`隔开。![需要修改的变量](https://github.com/iiunknown/sharepoint.powershell/raw/master/imgs/solution.deployment.1.png)
4. 在Powershell命令行窗口执行`solution.deployment.ps1`。![需要修改的变量](https://github.com/iiunknown/sharepoint.powershell/raw/master/imgs/solution.deployment.2.png)


### 脚本

```powershell
############################################################
#	ShuiShan S2 deployment script.
#	Version: 1.0.0.1229
#	8/29/2014
#	http://www.shuishan-tech.com
#	Author: YangLiu@上海水杉网络科技有限公司
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

#回收解决方案包。
function SSUninstallSolution($name)
{
	$solution = Get-SPSolution $name -ErrorAction Continue;
	if ($solution -eq $null -or !$solution.Deployed)
	{
		$(throw "Solution is null or the solution has not deployed, it can not be uninstalled.");
		exit;
	}
	Write-Output -InputObject "The solution: $solutionName is deployed in this farm.";
	Write-Output -InputObject "Prepare to uninstall solution: $solutionName";

	#获取解决方案包含的语言包。如果有语言包，则需要指定-AllWebApplications参数进行回收。
	$lanPack = $solution.GetLanguagePack(0);
	if ($lanPack -ne $null -and $lanPack.ContainsWebApplicationResource)
	{
		Uninstall-SPSolution -Identity $solution -AllWebApplications -Confirm:$false;
	}
	else {
		Uninstall-SPSolution -Identity $solution -Confirm:$false;
	}
	#$solution = Get-SPSolution $solutionName;
	while($solution.JobExists)
	{
		start-sleep 1
		Write-Output -InputObject "Uninstalling solution: $solutionName";
	}
	Write-Output -InputObject "Uninstalled solution: $solutionName";
}

#删除解决方案包。
function SSRemoveSolution($name)
{
	$solution = Get-SPSolution $name -ErrorAction Continue;
	if ($solution -eq $null -or $solution.Deployed)
	{
		$(throw "Solution is null or the solution has deployed, it can not be removed.");
		exit;
	}

	Write-Output -InputObject "Prepare to remove solution: $solutionName";
	Remove-SPSolution -Identity $solution -Confirm:$false;
	while($solution.JobExists)
	{
		start-sleep 1
		Write-Output -InputObject "Removing solution: $solutionName";
	}
	Write-Output -InputObject "Removed solution: $solutionName";
}

#部署解决方案包。
function SSInstallSolution($name)
{
	$solution = Get-SPSolution $name -ErrorAction Continue;
	if ($solution -eq $null -or $solution.Deployed)
	{
		$(throw "Solution is null or the solution has deployed, it can not be removed.");
		exit;
	}
	Write-Output -InputObject "Prepare to install solution: $name";
	#获取解决方案包含的语言包。如果有语言包，则需要推送到指定WebApplication或者全部的WebApplications
	$lanPack = $solution.GetLanguagePack(0);
	#获取解决方案包是否包含全局程序集，如果有，则需要使用-GACDeployment命令。
	$containsGlobalAssembly = $solution.ContainsGlobalAssembly;
	#解决方案包含语言包。
	if ($lanPack -ne $null -and $lanPack.ContainsWebApplicationResource)
	{
		if ($containsGlobalAssembly)
		{
			Install-SPSolution -Identity $solution -AllWebApplications -Force -GACDeployment -Confirm:$false;
		}
		else {
			Install-SPSolution -Identity $solution -AllWebApplications -Force -Confirm:$false;
		}

	}
	else
	{
		if ($containsGlobalAssembly)
		{
			Install-SPSolution -Identity $solution -Force -GACDeployment -Confirm:$false;
		}
		else {
			Install-SPSolution -Identity $solution -Force -Confirm:$false;
		}
	}
	$solution = Get-SPSolution $name;
	while($solution.JobExists)
	{
		start-sleep 1
		Write-Output -InputObject "Installing solution: $name";
	}
	Write-Output -InputObject "Installed solution: $name";
}

#需要部署的包列表，按照部署顺序使用分号隔开。
$solutionNames = "shuishan.s2.framework.wsp;shuishan.s2.organizationchart.wsp;shuishan.s2.form.wsp;shuishan.s2.mobile.wsp;shuishan.s2.workflow.wsp";
#获取脚本执行的路径。
$p = Get-Location;
#循环定义的解决方案包列表。
foreach($solutionName in $solutionNames.split(';'))
{
	$spath = $p.Path + "/" + $solutionName;
	$solution = Get-SPSolution $solutionName -ErrorAction Continue;
	#在当前场中找到解决方案。
	if ($solution -ne $null)
	{
		Write-Output -InputObject "The solution: $solutionName exists in this farm.";
		#判断解决方案是否已经部署。
		#解决方案已经部署过。
		if ($solution.Deployed)
		{
			#解决方案已经部署，需要先回收。
			SSUninstallSolution($solutionName);
		}

		#删除解决方案包。
		SSRemoveSolution($solutionName);
	}
	else
	{
		#在当前场中未找到解决方案，输出提示信息。
		Write-Output -InputObject "The solution: $solutionName is not exists in this farm.";
	}

	Write-Output -InputObject "Add solution: $solutionName";
	Add-SPSolution -LiteralPath $spath;
	SSInstallSolution($solutionName);
}

```
