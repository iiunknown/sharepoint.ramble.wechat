# PowerShell实战 第五回 创建HAB
    作者：小敏

HAB全称为hierarchical address book，中文名叫做分层通讯簿。可以用来展现您的组织架构。如下图：

![](imgs/150924-1.png)

今天要介绍的是如何使用PowerShell命令批量来创建HAB。

## 批量创建通讯组

`Import-Csv C:\GROUP.txt | foreach {New-DistributionGroup -Name $_.Name -PRIMARYSMTPAddress $_.PRIMARYSMTPAddress -alias $_.alias -OrganizationalUnit "contoso.com/小米科技"}`

其中GROUP.TXT的内容如下：

![](imgs/150924-2.png)


## 指定HAB 的根组织

`Set-OrganizationConfig -HierarchicalAddressBookRoot "小米科技"`


## 批量添加组成员

`Import-Csv C:\USERS.txt | foreach {add-distributiongroupmember "EBC.XHJR_RLZYB" -member $_.Name}`

其中user.TXT的内容如下：

![](imgs/150924-3.png)


## 启用HAB属性

`Set-Group -Identity "FM.XHJDLY_SHDCM" -IsHierarchicalGroup $true`


以上命令仅供参考，需要依据实际情况去调整。好了，我们下次再见。
