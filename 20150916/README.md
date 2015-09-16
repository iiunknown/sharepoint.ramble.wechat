# PowerShell实战 第三回 管理AD OU对象
    作者：小敏

Hello，大家好。今天来学习下powershell管理OU对象的相关命令


## 创建OU

`New-ADOrganizationalUnit -Name IT -Path "DC=Melody,DC=Net"`


## 重命名OU

`Rename-ADObject "OU=IT,DC=Melody,DC=Net" -NewName NewIT`

## 删除OU

`Remove-ADOrganizationalUnit IT –Recursive`

## 移动OU

`Get-ADObject -Filter {Name -Like "*"} -Searchbase "OU=IT,DC=Melody,DC=Net" -SearchScope OneLevel | Move-ADObject -TargetPath "OU=NewIT,DC=Melody,DC=Net"`

好了，今天的介绍就到这里。下次再见！
