# 如何修改管理中心端口号
	作者：jingnansu

有时候我们出于一些迫不得已的情况需要更改CA端口号, 我们可以使用PowerShell命令来达到我们的目的:      
Set-SPCentralAdministration -Port <PortNumber>      
注意这个命令需要有一定的权限才可以操作哦. 当然SharePoint 2007也有命令可以操作:      
stsadm -o setadminport -port <port> [-ssl] [-admapcreatenew] [-admapidname] <application pool name>      

enjoy SharePoint