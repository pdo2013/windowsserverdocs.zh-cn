---
title: 开发 Nano Server 的 PowerShell Cmdlet
description: '移植 CIM, NET cmdlet, C++ '
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b4267f0-1c91-4a40-9262-5daf4659f686
author: jaimeo
ms.author: jaimeo
ms.date: 09/06/2017
ms.localizationpriority: medium
ms.openlocfilehash: 4c669db414c4f12b6145a26a75b83449f43e8918
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887678"
---
# <a name="developing-powershell-cmdlets-for-nano-server"></a>开发 Nano Server 的 PowerShell Cmdlet

>适用于：Windows Server 2016

> [!IMPORTANT]
> 自 Windows Server 版本 1709 开始，Nano Server 将仅用作[容器基本操作系统映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)。 查看[对 Nano Server 进行的更改](nano-in-semi-annual-channel.md)以了解这意味着什么。 
  
## <a name="overview"></a>概述  
默认情况下，Nano Server 在所有 Nano Server 安装中都包括 PowerShell Core。 PowerShell Core 是基于 .NET Core 构建的 PowerShell 占用空间减小版本，在占用空间减小版本的 Windows （例如，Nano Server 和 Windows IoT Core）上运行。 PowerShell Core 与其他 PowerShell 版本（例如 Windows Server 2016 上运行的 Windows PowerShell）运行方式相同。 然而，Nano Server 占用空间减少意味着不是所有 Windows Server 2016 中的 PowerShell 功能都在 Nano Server 上的 PowerShell Core 中可用。  
  
如果要在 Nano Server 上运行现有 PowerShell cmdlet 或为此开发新的 PowerShell cmdlet，本主题中包含的提示和建议会使其更加简单。  

## <a name="powershell-editions"></a>PowerShell 版本  
  
  
从 5.1 版本开始，PowerShell 在具有不同功能集和平台兼容性的不同版本中可用。  
  
- **桌面版：**.NET Framework 上构建，并提供与面向新版的 Server Core 等的 Windows 和 Windows 桌面的完整占用空间减小版本上运行的 PowerShell 脚本和模块的兼容性。  
- **核心版：**.NET Core 上生成，并提供与面向版本的 Nano Server 等的 Windows 和 Windows IoT 的占用空间减少的版本上运行的 PowerShell 脚本和模块的兼容性。  
  
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
  
  
## <a name="installing-nano-server"></a>安装 Nano Server  
有关在虚拟或物理计算机上安装 Nano Server 的快速入门和详细步骤，请参阅本主题的父主题 - [安装 Nano Server](Getting-Started-with-Nano-Server.md)。  
  
> [!NOTE]  
> 使用 New-NanoServerImage 的 -Development 参数安装 Nano Server 可能会有助于 Nano Server 上的开发工作。 这将允许安装未签名的驱动程序、复制调试程序二进制、打开调试端口、启用测试签名和允许安装 AppX 程序包，且无需开发者许可证。 例如：  
>  
>`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`  
  
## <a name="determining-the-type-of-cmdlet-implementation"></a>决定 cmdlet 实现类型  
PowerShell 支持多种 cmdlet 实现类型，你使用的类型决定其创建或移植相关的过程和工具，以使其在 Nano Server 上运行。 支持的实现类型包括：  
* CIM - 由分层置放在 CIM (WMIv2) 提供程序上的 CDXML 文件组成   
* .NET - 由实现托管的 cmdlet 接口的 .NET 程序集组成，通常使用 C# 语言编写   
* PowerShell 脚本 - 由 PowerShell 语言编写的脚本模块 (.psm1) 或脚本 (.ps1) 组成   
  
如果不确定要移植的现有 cmdlet 已使用的实现，请安装产品或功能，然后在以下位置中查找 PowerShell 模块文件：   
  
* %windir%\system32\WindowsPowerShell\v1.0\Modules   
* %ProgramFiles%\WindowsPowerShell\Modules   
* %UserProfile%\Documents\WindowsPowerShell\Modules   
* \<产品安装位置 >   
    
 检查这些位置以获取详细信息：  
 * CIM cmdlet 具有.cdxml 文件扩展名。  
 * .NET cmdlet 具有.dll 文件扩展名，或具有安装到 RootModule、ModuleToProcess 或 NestedModules 字段下 .psd1 文件中所列出的 GAC 的程序集。  
* PowerShell 脚本 cmdlet 具有.psm1 或.ps1 文件扩展名。   
  
## <a name="porting-cim-cmdlets"></a>移植 CIM cmdlet  
通常情况下，这些 cmdlet 可在 Nano Server 中运行，无需任何转换。 但是，如果尚未进行此操作，则必须移植基础 WMI v2 提供程序以使其在 Nano Server 上运行。  
  
### <a name="building-c-for-nano-server"></a>生成适用于 Nano Server 的 C++  
若要使 C++ DLL 在 Nano Server 上运行，请将其编译为适用于 Nano Server 而非某特定版本。  
  
有关在 Nano Server 上开发 C++ 的先决条件和操作示例，请参阅 [Developing Native Apps on Nano Server](http://blogs.technet.com/b/nanoserver/archive/2016/04/27/developing-native-apps-on-nano-server.aspx)（在 Nano Server 上开发本机应用）。  
  
  
## <a name="porting-net-cmdlets"></a>移植 .NET cmdlet  
Nano Server 上支持大多数 C# 代码。 可使用 [ApiPort](https://github.com/Microsoft/dotnet-apiport) 扫描不兼容的 API。  
  
### <a name="powershell-core-sdk"></a>Powershell Core SDK  
模块“Microsoft.PowerShell.NanoServer.SDK”在 [PowerShell Gallery](https://www.powershellgallery.com/packages/Microsoft.PowerShell.NanoServer.SDK/)（PowerShell 库）中可通过 Visual Studio 2015 Update 2 用于简化面向 Nano Server 中可用 CoreCLR 和 PowerShell Core 版本的 .NET cmdlet 的开发。 可按照此命令使用 PowerShellGet 安装该模块：  
  
`Find-Module Microsoft.PowerShell.NanoServer.SDK -Repository PSGallery | Install-Module -Scope <scope>`  
  
PowerShell Core SDK 模块会暴露 cmdlet 来设置正确的 CoreCLR 和 PowerShell Core 引用程序集、在 Visual Studio 2015 中创建面向这些引用程序集的 C# 项目，并在 Nano Server 计算机上设置远程调试程序，从而使开发人员可在 Visual Studio 2015 中远程调试 Nano Server 上运行的 .NET cmdlet。  
  
PowerShell Core SDK 模块需要 Visual Studio 2015 Update 2。 如未安装 Visual Studio 2015，可安装 [Visual Studio Community 2015](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx)。  
  
该 SDK 模块还取决于 Visual Studio 2015 中要安装的以下功能：  
  
- Windows 和 Web 开发 -> 通用 Windows 应用开发工具 -> 工具 (1.3.1) 和 Windows 10 SDK  
  
使用此 SDK 模块之前请检查 Visual Studio 安装，以确保满足这些先决条件。 安装 Visual Studio 过程中，请务必选择安装上述功能，或修改现有 Visual Studio 2015 安装以进行安装。  
  
PowerShell Core SDK 模块包括以下 cmdlet：  
- New-NanoCSharpProject:创建新的 Visual StudioC#项目面向 CoreCLR 和 PowerShell Core 包含在 Windows Server 2016 版本的 Nano Server。  
- Show-SdkSetupReadMe:在文件资源管理器中打开 SDK 根文件夹并打开用于手动设置的 README.txt 文件。  
- Install-remotedebugger:安装和配置 Nano Server 计算机上的 Visual Studio 远程调试器。  
- Start-remotedebugger:在运行 Nano Server 的远程计算机上启动远程调试器。  
- 停止 RemoteDebugger:在运行 Nano Server 的远程计算机上停止远程调试器。  
  
有关如何使用这些 cmdlet 的详细信息，请在安装和导入该模块后，在每个 cmdlet 上运行 Get-Help，如下所示：  
  
`Get-Command -Module Microsoft.PowerShell.NanoServer.SDK | Get-Help -Full`   
  
  
### <a name="searching-for-compatible-apis"></a>搜索兼容的 API  
  
可在 API 目录中搜索 .NET Core，或反汇编 Core CLR 引用程序集。 有关 .NET API 平台可移植性的详细信息，请参阅 [Platform Portability](https://github.com/Microsoft/dotnet-apiport/blob/master/docs/HowTo/PlatformPortability.md)（平台可移植性）  
  
### <a name="pinvoke"></a>PInvoke  
在 Nano Server 使用的 Core CLR 中，kernel32.dll 和 advapi32.dll 等基础 DLL 已拆分为多个 API 集，因此需确保 PInvoke 引用了正确的 API。 任何不兼容性将表现为运行时错误。  
  
有关 Nano Server 上支持的本机 API 的列表，请参阅 [Nano 服务器 API](https://msdn.microsoft.com/library/mt588480(v=vs.85).aspx)。  
  
### <a name="building-c-for-nano-server"></a>生成适用于 Nano Server 的 C#  
  
通过使用 `New-NanoCSharpProject` 在 Visual Studio 2015 中创建 C# 项目后，只需单击“生成”菜单并选择“生成项目”或“生成解决方案”便可生成项目。 生成的程序集将面向 Nano Server 中正确的 CoreCLR 和 PowerShell Core，只需将这些程序集复制到运行 Nano Server 的计算机便可使用。  
  
### <a name="building-managed-c-cppcli-for-nano-server"></a>生成适用于 Nano Server 的托管 C++ (CPP/CLI)  
托管 C++ 不支持 CoreCLR。 移植到 CoreCLR 后，使用 C# 重新编写托管 C++ 代码，并通过 PInvoke 进行所有的本机调用。  
  
## <a name="porting-powershell-script-cmdlets"></a>移植 PowerShell 脚本 cmdlet  
  
PowerShell Core 具有其他 PowerShell 版本（包括 Windows Server 2016 和 Windows 10 中运行的版本）的完整 PowerShell 语言奇偶校验。 但是，将 PowerShell 脚本 cmdlet 移植到 Nano Server 时，请考虑这些因素：  
* 其他 cmdlet 上存在依赖关系吗？ 如果存在，这些 cmdlet 是否在 Nano Server 上可用？ 有关不可用内容的详细信息，请参阅 [Nano Server 上的 PowerShell](PowerShell-on-Nano-Server.md)。  
* 如果运行时加载的程序集上存在依赖关系，其是否仍然有效？   
* 如何远程调试脚本？   
* 如何从 WMI .Net 迁移到 MI .Net？  
  
### <a name="dependency-on-built-in-cmdlets"></a>内置 cmdlet 上的依赖关系  
并非所有 Windows Server 2016 中的 cmdlet 都在 Nano Server 上可用（请参阅 [Nano Server 上的 PowerShell](PowerShell-on-Nano-Server.md)）。 最佳方法是设置 Nano Server 虚拟机，然后检查所需 cmdlet 是否可用。 若要执行此操作，请运行 `Enter-PSSession` 以连接到目标 Nano Server，然后运行 `Get-Command -CommandType Cmdlet, Function` 获取可用 cmdlet 的列表。  
  
### <a name="consider-using-powershell-classes"></a>考虑使用 PowerShell 类  
Nano Server 上支持 Add-Type 编译内联 C# 代码。 如果要编写新代码或移植现有代码，也可考虑使用 PowerShell 类来定义自定义类型。 PowerShell 类可用于属性包方案和 Enums。 如需执行 PInvoke，请通过 C# 使用 Add-Type 或在预编译程序集中执行。  
演示 Add-Type 使用的示例如下：  
  
```  
Add-Type -ReferencedAssemblies ([Microsoft.Management.Infrastructure.Ciminstance].Assembly.Location) -TypeDefinition @'  
public class TestNetConnectionResult  
{  
   // The compute name  
   public string ComputerName = null;  
   // The Remote IP address used for connectivity  
   public System.Net.IPAddress RemoteAddress = null;  
}  
'@  
# Create object and set properties  
$result = New-Object TestNetConnectionResult  
$result.ComputerName = "Foo"  
$result.RemoteAddress = 1.1.1.1  
  
```  
 此示例演示在 Nano Server 上使用 PowerShell 类：  
   
```  
class TestNetConnectionResult    
{    
   # The compute name  
  [string] $ComputerName    
  
  #The Remote IP address used for connectivity    
  [System.Net.IPAddress] $RemoteAddress  
}  
# Create object and set properties  
$result = [TestNetConnectionResult]::new()  
$result.ComputerName = "Foo"  
$result.RemoteAddress = 1.1.1.1  
  
```  
  
### <a name="remotely-debugging-scripts"></a>远程调试脚本  
  
若要远程调试脚本，请使用 `Enter-PSsession` 从 PowerShell ISE 连接到远程计算机。 进入会话后，可运行 `psedit <file_path>`，这会在本地 PowerShell ISE 中打开该文件的副本。 然后，通过设置断点，可像本地运行脚本一样调试该脚本。 此外，对此文件所做的任何更改将保存在远程版本中。   
  
### <a name="migrating-from-wmi-net-to-mi-net"></a>从 WMI .NET 迁移到 MI .NET  
  
[WMI.NET](https://msdn.microsoft.com/library/mt481551(v=vs.110).aspx)不支持，因此，使用旧 API 的所有 cmdlet 必须在都迁移到受支持的 WMI API:[MI。NET](https://msdn.microsoft.com/library/dn387184(v=vs.85).aspx) 中。 可通过 C# 或 CimCmdlets 模块中的 cmdlet 直接访问 MI .NET。   
  
### <a name="cimcmdlets-module"></a>CimCmdlets 模块  
  
Nano Server 上不支持 WMI v1 cmdlet（例如，Get-WmiObject）。 但是，支持 CimCmdlets 模块中的 CIM cmdlet（例如，Get-CimInstance）。 CIM cmdlet 会非常紧密地映射到 WMI v1 cmdlet。 例如，Get-WmiObject 使用非常类似的参数与 Get-CimInstance 进行关联。 方法调用语法略有不同，但会通过 Invoke-CimMethod 详细记录。 注意参数键入。 MI .NET 在方法参数类型方面具有更严格的要求。  
  
### <a name="c-api"></a>C# API  
  
WMI .NET 包装 WMIv1 接口，而 MI .NET 包装 WMIv2 (CIM) 接口。 暴露的类可能不同，但基本操作非常相似。 枚举或获取对象实例，并在其上调用操作来完成任务。   
  
  


