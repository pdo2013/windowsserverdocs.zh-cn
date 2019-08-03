---
title: 管理 Nano Server
description: 更新, 服务包, 网络跟踪, 性能监视
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 599d6438-a506-4d57-a0ea-1eb7ec19f46e
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 165b7e7aea7a7d0bb56d21f350f6ee646d5fa973
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "67280407"
---
# <a name="manage-nano-server"></a>管理 Nano Server

>适用于：Windows Server 2016

> [!IMPORTANT]
> 自 Windows Server 版本 1709 开始，Nano Server 将仅用作[容器基本 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)。 查看[对 Nano Server 进行的更改](nano-in-semi-annual-channel.md)以了解这意味着什么。   

Nano Server 进行远程管理。 其不具备本地登录功能，亦不支持终端服务。 但是，有多种选项来远程管理 Nano Server，包括 Windows PowerShell、Windows Management Instrumentation (WMI)、Windows 远程管理和紧急管理服务 (EMS)。  

若要使用任何远程管理工具，可能需要知道 Nano Server 的 IP 地址。 了解 IP 地址的一些方法包括：  
  
-   使用 Nano 恢复控制台（请参阅本主题的“使用 Nano Server 恢复控制台”部分了解详细信息）。  
  
-   将串行电缆连接到计算机并使用 EMS。  
  
-   通过使用在配置 Nano Server 时分配给它的计算机名称，可以使用 ping 获取 IP 地址。 例如，`ping NanoServer-PC /4`。  
  
## <a name="using-windows-powershell-remoting"></a>使用 Windows PowerShell 远程控制  
若要使用 Windows PowerShell 远程控制管理 Nano Server，则需要将 Nano Server 的 IP 地址添加到受信任主机的管理计算机列表，将所使用的帐户添加到 Nano Server 的管理员，并启用 CredSSP（如果计划使用该功能）。  

> [!NOTE]
> 如果目标 Nano Server 和管理计算机处于相同的 AD DS 林中（或处于具有信任关系的林中），则不应将 Nano Server 添加到受信任的主机列表中 - 可以通过使用其完全限定的域名（例如：PS C:\>Enter-PSSession -ComputerName nanoserver.contoso.com -Credential (Get-Credential)）连接到 Nano Server
  
  
若要将 Nano Server 添加到受信任的主机列表，请在提升的 Windows PowerShell 提示符下运行此命令：  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
若要启动远程 Windows PowerShell 会话，请启动提升的本地 Windows PowerShell 会话，然后运行以下命令：  
  
  
```  
$ip = "<IP address of Nano Server>"  
$user = "$ip\Administrator"  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
现在可以在 Nano Server 上正常运行 Windows PowerShell 命令。  
  
> [!NOTE]  
> 并非所有的 Windows PowerShell 命令都在此版本的 Nano Server 中可用。 若要查看可用的命令，请运行 `Get-Command -CommandType Cmdlet`  
  
使用命令 `Exit-PSSession` 停止远程会话  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>通过 WinRM 使用 Windows PowerShell CIM 会话  
可以在 Windows PowerShell 中使用 CIM 会话和实例通过 Windows 远程管理 (WinRM) 来运行 WMI 命令。  
  
通过在 Windows PowerShell 提示符中运行以下命令启动 CIM 会话：  
  
  
```  
$ip = "<IP address of the Nano Server\>"  
$user = $ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
建立会话后，你可以运行各种 WMI 命令，例如：  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query "SELECT * from Win32_Process WHERE name LIKE 'p%'"  
```  
  
  
## <a name="windows-remote-management"></a>Windows 远程管理  
可以使用 Windows 远程管理 (WinRM) 在 Nano Server 上远程运行程序。 若要使用 WinRM，首先配置服务并在提升的命令提示符下使用以下命令设置代码页：  
  
```
winrm quickconfig
winrm set winrm/config/client @{TrustedHosts="<ip address of Nano Server>"}
chcp 65001
```
  
现在可以在 Nano Server 上远程运行命令。 例如：  

```
winrs -r:<IP address of Nano Server> -u:Administrator -p:<Nano Server administrator password> ipconfig
```
  
有关 Windows 远程管理的详细信息，请参阅 [Windows 远程管理 (WinRM) 概述](https://technet.microsoft.com/library/dn265971.aspx)。  
   
   
  
## <a name="running-a-network-trace-on-nano-server"></a>在 Nano Server上运行网络跟踪  
 Nano Server 中不提供 Netsh trace、Tracelog.exe 和 Logman.exe。 若要捕获网络数据包，可以使用这些 Windows PowerShell cmdlet：  
   
   
```  
New-NetEventSession [-Name]  
Add-NetEventPacketCaptureProvider -SessionName  
Start-NetEventSession [-Name]  
Stop-NetEventSession [-Name]  
```  
[Windows PowerShell 中的网络事件数据包捕获 Cmdlet](https://technet.microsoft.com/library/dn268520(v=wps.630).aspx) 详细记录了这些 cmdlet  

## <a name="installing-servicing-packages"></a>安装服务包  
如果想要安装服务包，请使用 -ServicingPackagePath 参数（可以向 .cab 文件传递一系列路径）：  
  
`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -ServicingPackagePath \\path\to\kb123456.cab`  
  
通常情况下，服务包或修补程序被下载为一个包含 .cab 文件的 KB 项。 请按照下列步骤来提取 .cab 文件，随后可以使用 -ServicingPackagePath 参数安装该文件：  
  
1.  下载服务包（从关联的知识库文章或从 [Microsoft 更新目录](https://catalog.update.microsoft.com/v7/site/home.aspx)）。 将其保存到本地目录或网络共享，例如：C:\ServicingPackages  
2.  创建将在其中保存提取的服务包的文件夹。  示例：c:\KB3157663_expanded  
3.  打开 Windows PowerShell 控制台，然后使用 `Expand` 命令指定到服务包的 .msu 文件的路径，包括 `-f:*` 参数和将服务包提取到的路径。  例如：`Expand "C:\ServicingPackages\Windows10.0-KB3157663-x64.msu" -f:* "C:\KB3157663_expanded"`  
  
    展开的文件应如下所示：  
C:>dir C:\KB3157663_expanded   
驱动器 C 中的卷是操作系统  
卷序列号是 B05B-CC3D  
   
      C:\KB3157663_expanded 的目录  
   
      04/19/2016  01:17 PM    \<DIR>  
      04/19/2016  01:17 PM    \<DIR>  
        04/17/2016  12:31 AM               517 Windows10.0-KB3157663-x64-pkgProperties.txt  
04/17/2016  12:30 AM        93,886,347 Windows10.0-KB3157663-x64.cab  
04/17/2016  12:31 AM               454 Windows10.0-KB3157663-x64.xml  
04/17/2016  12:36 AM           185,818 WSUSSCAN.cab  
               4 个文件     94,073,136 字节  
               2 个目录  328,559,427,584 字节可用  
4.  结合指向此目录中的 .cab 文件的 -ServicingPackagePath 参数运行 `New-NanoServerImage`，例如：`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -ServicingPackagePath C:\KB3157663_expanded\Windows10.0-KB3157663-x64.cab`  

## <a name="managing-updates-in-nano-server"></a>管理 Nano Server 中的更新

当前可以使用 Windows Management Instrumentation (WMI) 的 Windows 更新提供程序找到合适的更新列表，然后安装所有或其子集。 如果使用 Windows Server Update Services (WSUS)，还可以配置 Nano Server 来联系 WSUS 服务器以获取更新。  

在所有情况下，首先建立与 Nano Server 计算机的远程 Windows PowerShell 会话。 这些示例使用 *$sess* 进行会话；如果使用其他内容，请根据需要替换该元素。  


### <a name="view-all-available-updates"></a>查看所有可用的更新  
---  
使用以下命令获取适用更新的完整列表：  
```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}  
```  
**注意：**  
如果没有更新可用，则此命令将返回以下错误：  
```  
Invoke-CimMethod : A general error occurred that is not covered by a more specific error code.  

At line:1 char:16  

+ ... anResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUp ...  

+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  

    + CategoryInfo          : NotSpecified: (MSFT_WUOperatio...-5b842a3dd45d")  

   :CimInstance) [Invoke-CimMethod], CimException  

    + FullyQualifiedErrorId : MI RESULT 1,Microsoft.Management.Infrastructure.  

   CimCmdlets.InvokeCimMethodCommand  
```  

### <a name="install-all-available-updates"></a>安装所有可用更新  
---  
通过使用以下命令可以一次性检测、下载和安装**所有**可用更新：  

```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ApplyApplicableUpdates  

Restart-Computer  
```  
**注意：**  
Windows Defender 将阻止安装更新。 若要解决此问题，卸载 Windows Defender、安装更新，然后重新安装 Windows Defender。 或者，可以在另一台计算机上下载更新，将更新复制到 Nano Server 上，然后使用 DISM.exe 应用更新。  


### <a name="verify-installation-of-updates"></a>验证安装的更新  
---  
使用这些命令来获取当前安装的更新列表：  
```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}  
```  

**注意：**  
这些命令将列出已安装的内容，但不会在输出中专门引用“安装”。 如果需要包括“安装”的输出，例如对于报表，则可以运行  
```  
Get-WindowsPackage--Online  
```  

### <a name="using-wsus"></a>使用 WSUS  
---  
上面列出的命令将通过 Internet 查询 Windows 更新和 Microsoft 更新服务来查找并下载更新。 如果使用 WSUS，可以在 Nano Server 上设置注册表项来改为使用 WSUS 服务器。  
  
请参阅[在非 Active Directory 环境中配置自动更新](https://technet.microsoft.com/library/cc708449(v=ws.10).aspx)中的“Windows 更新代理环境选项注册表项”表  
  
应至少设置 **WUServer** 和 **WUStatusServer** 注册表项，但是具体取决于如何实现 WSUS，可能会需要其他值。 始终可以通过检查同一环境中的另一台 Windows Server 来确认这些设置。  

为 WSUS 设置这些值后，以上部分中的命令将查询该服务器的更新，并将其作为下载源。  

### <a name="automatic-updates"></a>自动更新  
---  
目前，自动执行更新安装的方法是将上述步骤转换为本地 Windows PowerShell 脚本，然后创建计划的任务来运行该脚本并按计划重新启动系统。


## <a name="performance-and-event-monitoring-on-nano-server"></a>在 Nano Server 上进行性能和事件监视
[comment]: # (从 Venkat Yalla。)
Nano Server 完全支持 [Windows 事件跟踪](https://aka.ms/u2pa0i) (ETW) 框架，但一些用于管理跟踪的熟悉工具和性能计数器当前在 Nano Server 上不可用。 但是，Nano Server 具有的工具和 cmdlet 可完成最常见的性能分析方案。

高级工作流保持与在任何 Window Server 安装上相同的状态，即在目标 (Nano Server) 计算机上执行低开销水平跟踪，并且生成的跟踪文件和/或日志在单独的计算机上使用 [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx)、[消息分析器](https://www.microsoft.com/download/details.aspx?id=44226)等脱机进行后期处理。

> [!NOTE]
> 请参阅[如何将文件复制到 Nano Server 上以及反向复制](https://aka.ms/nri9c8)，以复习一下如何使用 PowerShell 远程控制传送文件。

以下部分列出了最常见的性能数据收集活动以及在 Nano Server 上实现这些活动的受支持的方法。

### <a name="query-available-event-providers"></a>查询可用事件提供程序
[Windows Performance Recorder](https://msdn.microsoft.com/library/hh448229.aspx) 是用于查询可用事件提供程序的工具，如下所示：
```
wpr.exe -providers
```

可以按照感兴趣的事件类型筛选输出。 例如：
```
PS C:\> wpr.exe -providers | select-string "Storage"

       595f33ea-d4af-4f4d-b4dd-9dacdd17fc6e                              : Microsoft-Windows-StorageManagement-WSP-Host
       595f7f52-c90a-4026-a125-8eb5e083f15e                              : Microsoft-Windows-StorageSpaces-Driver
       69c8ca7e-1adf-472b-ba4c-a0485986b9f6                              : Microsoft-Windows-StorageSpaces-SpaceManager
       7e58e69a-e361-4f06-b880-ad2f4b64c944                              : Microsoft-Windows-StorageManagement
       88c09888-118d-48fc-8863-e1c6d39ca4df                              : Microsoft-Windows-StorageManagement-WSP-Spaces
```

### <a name="record-traces-from-a-single-etw-provider"></a>记录来自单个 ETW 提供程序的跟踪
可以使用新的[事件跟踪管理 cmdlet](https://technet.microsoft.com/library/dn919247.aspx) 进行此操作。 下面是示例工作流：

创建并启动跟踪，同时指定存储事件的文件名。
```
PS C:\> New-EtwTraceSession -Name "ExampleTrace" -LocalFilePath c:\etrace.etl
```

将提供程序 GUID 添加到跟踪。 将 ```wpr.exe -providers``` 用于进行 GUID 转换的提供程序名称。 
```
PS C:\> wpr.exe -providers | select-string "Kernel-Memory"

       d1d93ef7-e1f2-4f45-9943-03d245fe6c00                              : Microsoft-Windows-Kernel-Memory

PS C:\> Add-EtwTraceProvider -Guid "{d1d93ef7-e1f2-4f45-9943-03d245fe6c00}" -SessionName "ExampleTrace"
```

删除跟踪，此操作可以停止跟踪会话，同时将事件刷新到相关联的日志文件。
```
PS C:\> Remove-EtwTraceSession -Name "ExampleTrace"

PS C:\> dir .\etrace.etl

    Directory: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/14/2016  11:17 AM       16515072 etrace.etl
```
> [!NOTE]
> 此示例显示将单个跟踪提供程序添加到会话，但是也可以在具有不同提供程序 GUID 的跟踪会话上多次使用 ```Add-EtwTraceProvider``` cmdlet，以启用来自多个源的跟踪。 另一种方法是如下所述使用 ```wpr.exe``` 配置文件。

### <a name="record-traces-from-multiple-etw-providers"></a>记录来自多个 ETW 提供程序的跟踪
[Windows Performance Recorder](https://msdn.microsoft.com/library/hh448229.aspx) 的 ```-profiles``` 选项可实现同时从多个提供程序的跟踪。 有多个内置的配置文件，如 CPU、网络和 DiskIO 可供选择：
```
PS C:\Users\Administrator\Documents> wpr.exe -profiles 

Microsoft Windows Performance Recorder Version 10.0.14393 (CoreSystem)
Copyright (c) 2015 Microsoft Corporation. All rights reserved.

        GeneralProfile              First level triage
        CPU                         CPU usage
        DiskIO                      Disk I/O activity
        FileIO                      File I/O activity
        Registry                    Registry I/O activity
        Network                     Networking I/O activity
        Heap                        Heap usage
        Pool                        Pool usage
        VirtualAllocation           VirtualAlloc usage
        Audio                       Audio glitches
        Video                       Video glitches
        Power                       Power usage
        InternetExplorer            Internet Explorer
        EdgeBrowser                 Edge Browser
        Minifilter                  Minifilter I/O activity
        GPU                         GPU activity
        Handle                      Handle usage
        XAMLActivity                XAML activity
        HTMLActivity                HTML activity
        DesktopComposition          Desktop composition activity
        XAMLAppResponsiveness       XAML App Responsiveness analysis
        HTMLResponsiveness          HTML Responsiveness analysis
        ReferenceSet                Reference Set analysis
        ResidentSet                 Resident Set analysis
        XAMLHTMLAppMemoryAnalysis   XAML/HTML application memory analysis
        UTC                         UTC Scenarios
        DotNET                      .NET Activity
        WdfTraceLoggingProvider     WDF Driver Activity
```

有关创建自定义配置文件的详细指导，请参阅 [WPR.exe 文档](https://msdn.microsoft.com/library/windows/hardware/hh448223.aspx)。

### <a name="record-etw-traces-during-operating-system-boot-time"></a>在操作系统启动时记录 ETW 跟踪
使用 ```New-AutologgerConfig``` cmdlet 在系统启动期间收集事件。 用法与 ```New-EtwTraceSession``` cmdlet 非常类似，但添加到自动记录器的配置的提供程序仅在下次启动早些时候启用。 整个工作流如下所示：

首先，创建新的自动记录器配置。
```
PS C:\> New-AutologgerConfig -Name "BootPnpLog" -LocalFilePath c:\bootpnp.etl 
```

向其添加 ETW 提供程序。 此示例使用内核即插即用提供程序。 再次调用 ```Add-EtwTraceProvider```，同时指定相同的自动记录器名称，但指定不同的 GUID，以启用多个来源的启动跟踪收集。
```
Add-EtwTraceProvider -Guid "{9c205a39-1250-487d-abd7-e831c6290539}" -AutologgerName BootPnpLog
```

这不会立即开始 ETW 会话，而是将会话配置为在下次启动时开始。 重新启动后，具有自动记录器配置名称的新的 ETW 会话自动开始，同时会启用添加的跟踪提供程序。 Nano Server 启动后，将记录的事件刷新到关联的跟踪文件后，以下命令将停止跟踪会话：
```
PS C:\> Remove-EtwTraceSession -Name BootPnpLog
```

若要防止在下次启动时自动创建另一个跟踪会话，请按如下方式删除自动记录器配置：
```
PS C:\> Remove-AutologgerConfig -Name BootPnpLog
```

若要跨多系统或在无盘系统上收集启动和安装跟踪，请考虑使用[安装和启动事件收集](../administration/get-started-with-setup-and-boot-event-collection.md)。

### <a name="capture-performance-counter-data"></a>捕获性能计数器数据
通常可以使用 Perfmon.exe GUI 监视性能计数器数据。 在 Nano Server 上，使用 ```Typeperf.exe``` 命令行等效项。 例如：

查询可用的计数器 - 可以筛选输出以轻松找到感兴趣的计数器。
```
PS C:\> typeperf.exe -q | Select-String "UDPv6"

\UDPv6\Datagrams/sec
\UDPv6\Datagrams Received/sec
\UDPv6\Datagrams No Port/sec
\UDPv6\Datagrams Received Errors
\UDPv6\Datagrams Sent/sec
```

选项允许指定收集计数器值的次数和时间间隔。 在下面的示例中，每隔 3 秒收集 5 次处理器空闲时间。
```
PS C:\> typeperf.exe "\Processor Information(0,0)\% Idle Time" -si 3 -sc 5

"(PDH-CSV 4.0)","\\ns-g2\Processor Information(0,0)\% Idle Time"
"09/15/2016 09:20:56.002","99.982990"
"09/15/2016 09:20:59.002","99.469634"
"09/15/2016 09:21:02.003","99.990081"
"09/15/2016 09:21:05.003","99.990454"
"09/15/2016 09:21:08.003","99.998577"
Exiting, please wait...
The command completed successfully.
```

使用其他命令行选项可以指定配置文件中感兴趣的性能计数器名称，同时将输出重定向到日志文件等。 请参阅 [typeperf.exe 文档](https://technet.microsoft.com/library/bb490960.aspx) 了解详细信息。

还可以远程结合使用 Perfmon.exe 的图形界面和 Nano Server 目标。 当向视图添加性能计数器时，在计算机名称中指定 Nano Server 目标，而不是默认的 *<Local computer>* 。

### <a name="interact-with-the-windows-event-log"></a>与 Windows 事件日志进行交互

Nano Server 支持 ```Get-WinEvent``` cmdlet，后者可在本地以及远程计算机上提供 Windows 事件日志筛选和查询功能。 详细的选项和示例可在 [Get-WinEvent 文档页](https://technet.microsoft.com/library/hh849682.aspx) 中找到。 这个简单的示例检索了过去两天内在*系统*日志中发现的*错误*。
```
PS C:\> $StartTime = (Get-Date) - (New-TimeSpan -Day 2)
PS C:\> Get-WinEvent -FilterHashTable @{LogName='System'; Level=2; StartTime=$StartTime} | select TimeCreated, Message

TimeCreated           Message
-----------           -------
9/15/2016 11:31:19 AM Task Scheduler service failed to start Task Compatibility module. Tasks may not be able to reg...
9/15/2016 11:31:16 AM The Virtualization Based Security enablement policy check at phase 6 failed with status: {File...
9/15/2016 11:31:16 AM The Virtualization Based Security enablement policy check at phase 0 failed with status: {File...
```

Nano Server 还支持 ```wevtutil.exe```，后者允许检索有关事件日志和发布程序的信息。 请参阅 [wevtutil.exe 文档](https://aka.ms/qvod7p) 了解更多详细信息。 

### <a name="graphical-interface-tools"></a>图形界面工具
[基于 Web 的服务器管理工具](https://blogs.technet.microsoft.com/servermanagement/2016/08/17/deploy-setup-server-management-tools/) 可以用于远程管理 Nano Server 目标和使用 Web 浏览器显示 Nano Server 事件日志。 最后，MMC 管理单元事件查看器 (eventvwr.msc) 还可以用于查看日志，只需通过桌面在计算机上打开它并使其指向远程 Nano Server。




## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>结合使用 Windows PowerShell Desired State Configuration 与 Nano Server  
  
可以使用 Windows PowerShell Desired State Configuration (DSC) 将 Nano Server 作为目标节点来管理。 目前，仅可以在请求模式下管理使用 DSC 运行 Nano Server 的节点。 并非所有 DSC 功能都与 Nano Server 正常运行。  
  
有关完整的详细信息，请参阅[使用 Nano Server 上的 DSC](https://msdn.microsoft.com/powershell/dsc/nanoDsc)。  
