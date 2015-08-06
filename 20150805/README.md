# PowerShell介绍 第七回 变量
    作者：小敏


了解Windows Power Shell的变量和常量，是灵活编写脚本程序的基础，那么本节我们就来了解下变量吧。

在 Windows PowerShell 中，有几种不同类型的变量：


1. 用户创建的变量：用户创建的变量由用户创建和维护。默认情况下，在 Windows PowerShell 命令行中创建的变量只在 Windows PowerShell 窗口打开时存在。关闭该窗口后，变量也不再存在。


1. 自动变量：自动变量存储 Windows PowerShell 的状态。这些变量由 Windows PowerShell 创建，       Windows PowerShell 根据需要更改变量值以保持其准确性。用户不能更改这些变量的值。


1. 首选项变量：首选项变量存储 Windows PowerShell 的用户首选项。这些变量由 Windows PowerShell       创建，并以默认值填充。用户可以更改这些变量的值。例如，MaximumHistoryCount 可确定会话历史记录中的最大条目数。


1. 此外，除了自动变量之外，在声明变量时，要回避一些特殊名称，这些名称被称作“系统保留字”，如下：
break | continue | do | else | elseif | filter | foreach | function | if | in | return | switch | until | where | while


## 自变量




- $$：包含会话所收到的最后一行中的最后一个令牌。

- $? ：包含最后一个操作的执行状态。如果最后一个操作成功，则包含 TRUE，失败则包含 FALSE。
 
- $^：包含会话所收到的最后一行中的第一个令牌。
 
- $_：包含管道对象中的当前对象。在对管道中的每个对象或所选对象执行操作的命令中，可以使用此变量。$WMI = Get-WmiObject -List | Where-Object {$_.name -Match “win32_operatingsystem”}
 
- $Args：包含由未声明参数和/或传递给函数、脚本或脚本块的参数值组成的数组。
在创建函数时可以声明参数，方法是使用 param 关键字或在函数名称后添加以圆括号括起、逗号分隔的参数列表。

- $ConsoleFileName：
包含在会话中最近使用的控制台文件 (.psc1) 的路径。在通过 PSConsoleFile 参数启动 Windows PowerShell 或使用 Export-Console cmdlet 将管理单元名称导出到控制台文件时，将填充此变量。

- $Error： 
包含错误对象的数组，这些对象表示最近的一些错误。最近的错误是该数组中的第一个错误对象  ($Error[0])。

- $Event：
包含一个 PSEventArgs 对象，该对象表示一个正在被处理的事件。 
此变量只在事件注册命令（例如 Register-ObjectEvent）的 Action 块内填充。 此变量的值是 Get-Event cmdlet 返回的同一个对象。 因此，可以在 Action 脚本块中使用 $Event 变量的属性（例如$Event.TimeGenerated）。 

- $EventSubscriber：
包含一个 PSEventSubscriber 对象，该对象表示正在被处理的事件的事件订阅者。 此变量只在事件注册命令的 Action 块内填充。此变量的值 是 Get-EventSubscriber cmdlet 返回的同一个对象。
 


- $ExecutionContext：
包含一个 EngineIntrinsics 对象，该对象表示 Windows PowerShell 主机的执行上下文。可以使用此变量来查找可用于 cmdlet 的执行对象。


- $False：
包含 FALSE。可以使用此变量在命令和脚本中表示 FALSE，而不是使用字符串"false"。如果该字符串转换为非空字符串或非零整数，则可将该字符串解释为 TRUE。

- $ForEach：
包含 ForEach-Object 循环的枚举数。可以对 $ForEach 变量的值使用枚举数的属性和方法。此变量仅在运行 For 循环时存在，循环完成即会删除。

- $Home
包含用户的主目录的完整路径。此变量等效于 %homedrive%%homepath% 环境变量。

- $Host
包含一个对象，该对象表示 Windows PowerShell 的当前主机应用程序。可以使用此变量在命令中表示当前主机，或者显示或更改主机的属性，如 $Host.version、$Host.CurrentCulture 或 $host.ui.rawui.setbackgroundcolor("Red")。

- $Input
一个枚举数，它包含传递给函数的输入。$Input 变量区分大小写，只能用于函数和脚本块。（脚本块本质上是未命名的函数。）在函数的 Process 块中，$Input 变量包含当前位于管道中的对象。在 Process 块完成后，$Input 的值为 NULL。如果函数没有 Process 块，则 $Input 的值可用于 End 块，它包含函数的所有输入。


- $LastExitCode
包含运行的最后一个基于 Windows 的程序的退出代码。

- $Matches变量与 -match 和 -not match 运算符一起使用。将标量输入提交给 -match 或 -notmatch 运算符时，如果检测到匹配，则会返回一个布尔值，并使用由所有匹配字符串值组成的哈希表填充 $Matches 自动变量。

- $MyInvocation
包含一个对象，该对象具有有关当前命令（如脚本、函数或脚本块）的信息。可以使用该对象中的信息（如脚本的路径和文件名 ($myinvocation.mycommand.path) 或函数的名称  ($myinvocation.mycommand.name)）来标识当前命令。对于查找正在运行的脚本的名称，这非常有用。


- $NULL
包含 NULL 或空值。可以在命令和脚本中使用此变量表示 NULL，而不是使用字符串"NULL"。如果该字符串转换为非空字符串或非零整数，则可将该字符串解释为 TRUE。

- $PID
包含承载当前 Windows PowerShell 会话的进程的进程标识符 (PID)。

- $Profile
包含当前用户和当前主机应用程序的 Windows PowerShell 配置文件的完整路径。可以在命令中使用此变量表示配置文件。


- $PsCmdlet
包含一个对象，该对象表示正在运行的 cmdlet 或高级函数。

- $PsCulture
包含操作系统中当前所用的区域性的名称。

- $PsHome
包含 Windows PowerShell 的安装目录的完整路径（通常为 %windir%\System32\WindowsPowerShell\v1.0）。可以在 Windows PowerShell 文件的路径中使用此变量。

- $PSScriptRoot
包含要从中执行脚本模块的目录。通过此变量，脚本可以使用模块路径来访问其他资源。

- $PsUICulture
包含操作系统中当前所用的用户界面 (UI) 区域性的名称。UI 区域性确定哪些文本字符串用于用户界面元素（如菜单和消息）。这是系统的System.Globalization.CultureInfo.CurrentUICulture.Name 属性的值。要获取系统的 System.Globalization.CultureInfo 对象，请使用 Get-UICulture cmdlet。

- $PsVersionTable
包含一个只读哈希表，该哈希表显示有关在当前会话中运行的 Windows PowerShell 版本的详细信息。

- $Pwd
包含一个路径对象，该对象表示当前目录的完整路径。


- $Sender 
包含生成此事件的对象。此变量只在事件注册命令的 Action 块内填充。 此变量的值也可在 Get-Event 返回的 PSEventArgs (System.Management.Automation.PSEventArgs) 对象的 Sender 属性中找到。 

- $ShellID
包含当前 shell 的标识符。


- $SourceArgs 
包含表示正在被处理的事件的事件参数的对象。此变量只在事件注册命令的 Action 块内填充。此变量的值也可在 Get-Event 返回的 PSEventArgs  (System.Management.Automation.PSEventArgs) 对象的 SourceArgs 属性中找到。 


- $SourceEventArgs 
包含一个对象，该对象表示从正在被处理的事件的 EventArgs 中派生出的第一个事件参数。此变量只在事件注册命令的 Action 块内填充。 此变量的值也可在 Get-Event 返回的 PSEventArgs (System.Management.Automation.PSEventArgs) 对象的 SourceArgs 属性中找到。


- $This
在定义脚本属性或脚本方法的脚本块中，$This 变量引用要扩展的对象。


- $True
包含 TRUE。可以在命令和脚本中使用此变量表示 TRUE。

好了，今天的介绍就到这里。下次再见
