---
title: Nano Server 上的 PowerShell
description: Nano Server 上减少的 PowerShell 功能集中的差异
ms.prod: windows-server
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b25b939-1e2c-4bed-a8d3-2a8e8e46b53d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: f662f1f48e903fac69c85185222c08954c5994a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360264"
---
# <a name="powershell-on-nano-server"></a>Nano Server 上的 PowerShell

>适用于：Windows Server 2016
  
> [!IMPORTANT]
> 自 Windows Server 版本 1709 开始，Nano Server 将仅用作[容器基本 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)。 查看[对 Nano Server 进行的更改](nano-in-semi-annual-channel.md)以了解其含义。 
  
## <a name="powershell-editions"></a>PowerShell 版本   
  
从 5.1 版本开始，PowerShell 在具有不同功能集和平台兼容性的不同版本中可用。  
  
- **桌面版：** 基于 .NET Framework 而构建，兼容面向在 Windows 完整占用空间版本（例如，Server Core 和 Windows Desktop）上运行的 PowerShell 版本的脚本和模块。  
- **核心版：** 基于 .NET Core 而构建，兼容面向在 Windows 占用空间减小版本（例如，Nano Server 和 Windows IoT）上运行的 PowerShell 版本的脚本和模块。  
  
当前运行的 PowerShell 版本显示在 $PSVersionTable 的 PSEdition 属性中。  
```powershell  
$PSVersionTable  
  
Name                           Value  
----                           -----  
PSVersion                      5.1.14300.1000  
PSEdition                      Desktop  
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}  
CLRVersion                     4.0.30319.42000  
BuildVersion                   10.0.14300.1000  
WSManStackVersion              3.0  
PSRemotingProtocolVersion      2.3  
SerializationVersion           1.1.0.1  
```  
  
模块作者可使用 CompatiblePSEditions 模块清单秘钥声明模块兼容一个或多个 PowerShell 版本。 仅 PowerShell 5.1 或更高版本支持此密钥。  
```powershell  
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion 5.1  
$moduleInfo = Test-ModuleManifest -Path \TestModuleWithEdition.psd1  
$moduleInfo.CompatiblePSEditions  
Desktop  
Core  
  
$moduleInfo | Get-Member CompatiblePSEditions  
  
   TypeName: System.Management.Automation.PSModuleInfo  
  
Name                 MemberType Definition  
----                 ---------- ----------  
CompatiblePSEditions Property   System.Collections.Generic.IEnumerable[string] CompatiblePSEditions {get;}  
  
```  
获取可用模块列表后，可按 PowerShell 版本筛选列表。  
```powershell  
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains "Desktop"  
  
    Directory: C:\Program Files\WindowsPowerShell\Modules  
  
  
ModuleType Version    Name                                ExportedCommands  
---------- -------    ----                                ----------------  
Manifest   1.0        ModuleWithPSEditions  
  
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains "Core" | % CompatiblePSEditions  
Desktop  
Core  
  
```  
脚本作者可在 #requires 语句中使用 PSEdition 参数来阻止脚本执行，除非其在兼容的 PowerShell 版本上运行。  
```powershell  
Set-Content C:\script.ps1 -Value "#requires -PSEdition Core  
Get-Process -Name PowerShell"  
Get-Content C:\script.ps1  
#requires -PSEdition Core  
Get-Process -Name PowerShell  
  
C:\script.ps1  
C:\script.ps1 : The script 'script.ps1' cannot be run because it contained a "#requires" statement for PowerShell editions 'Core'. The edition of PowerShell that is required by the script does not match the currently running PowerShell Desktop edition.  
At line:1 char:1  
+ C:\script.ps1  
+ ~~~~~~~~~~~~~  
    + CategoryInfo          : NotSpecified: (script.ps1:String) [], RuntimeException  
    + FullyQualifiedErrorId : ScriptRequiresUnmatchedPSEdition  
```  
  
## <a name="differences-in-powershell-on-nano-server"></a>Nano Server 上 PowerShell 的差异  
默认情况下，Nano Server 在所有 Nano Server 安装中都包括 PowerShell Core。 PowerShell Core 是基于 .NET Core 构建的 PowerShell 占用空间减小版本，且在占用空间减小版本的 Windows（例如，Nano Server 和 Windows IoT Core）上运行。 PowerShell Core 与其他 PowerShell 版本（例如 Windows Server 2016 上运行的 Windows PowerShell）运行方式相同。 然而，Nano Server 占用空间减少意味着不是所有 Windows Server 2016 中的 PowerShell 功能都在 Nano Server 上的 PowerShell Core 中可用。  
  
  
**Nano Server 中不可用的 Windows PowerShell 功能**  
* ADSI、ADO 以及 WMI 类型适配器   
* Enable-PSRemoting、Disable-PSRemoting（默认启用 PowerShell 远程处理；请参阅[安装 Nano Server](Getting-Started-with-Nano-Server.md) 的“使用Windows PowerShell 远程处理”部分）。  
* 计划作业和 PSScheduledJob 模块   
* 用于加入域的计算机 cmdlet { Add | Remove }（有关将 Nano Server 加入域的其他方法，请参阅[安装 Nano Server](Getting-Started-with-Nano-Server.md) 的“将 Nano Server 加入域”部分）。  
* Reset-ComputerMachinePassword, Test-ComputerSecureChannel   
* 配置文件（你可以添加启动脚本，实现与 `Set-PSSessionConfiguration` 的传入远程连接）  
* 剪贴板 cmdlet   
* EventLog cmdlet { Clear | Get | Limit | New | Remove | Show | Write }（改用 New-WinEvent 和 Get-WinEvent cmdlets）。   
* Get-PfxCertificate cmdlet   
* TraceSource cmdlet { Get | Set }   
* 计数器 cmdlet { Get | Export | Import }   
* 一些与 Web 相关的 cmdlet { New-WebServiceProxy、Send-MailMessage、ConvertTo-Html }  
* 使用 PSDiagnostics 模块进行日志记录和跟踪    
* Get-HotFix（若要获取并管理 Nano Server 的更新，请参阅[管理 Nano Server](Manage-Nano-Server.md)）。  
* 隐式远程处理 cmdlet { Export-PSSession | Import-PSSession }   
* New-PSTransportOption   
* PowerShell 事务和事务 cmdlet { Complete | Get | Start | Undo | Use }   
* PowerShell 工作流基础结构、模块和 cmdlet   
* Out-printer   
* Update-List   
* WMI v1 cmdlet：Get-WmiObject、Invoke-WmiMethod、Register-WmiEvent、Remove-WmiObject、Set-WmiInstance（改用 CimCmdlet 模块。）   
  
## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>结合使用 Windows PowerShell Desired State Configuration 与 Nano Server  
  
可以使用 Windows PowerShell Desired State Configuration (DSC) 将 Nano Server 作为目标节点来管理。 目前，仅可以在请求模式下管理使用 DSC 运行 Nano Server 的节点。 并非所有 DSC 功能都与 Nano Server 正常运行。  
  
有关完整的详细信息，请参阅[使用 Nano Server 上的 DSC](https://msdn.microsoft.com/powershell/dsc/nanoDsc)。  
  
  


