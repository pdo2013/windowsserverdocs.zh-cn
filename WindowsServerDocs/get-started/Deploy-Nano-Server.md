---
title: 部署 Nano Server
description: 介绍如何创建和部署自定义映像、包、驱动程序、域、角色、功能
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9f109c91-7c2e-4065-856c-ce9e2e9ce558
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 8953627b7240bdb6bd0ea76b09c5af8317bde186
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121496"
---
# 部署 Nano Server

>适用于：Windows Server 2016

> [!IMPORTANT]
> 自 Windows Server 版本 1709 开始，Nano Server 将仅用作[容器基本操作系统映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)。 查看[对 Nano Server 进行的更改](nano-in-semi-annual-channel.md)以了解这意味着什么。 

本主题介绍部署 Nano Server 映像时所需的信息，这些映像相较于 Nano Server 快速入门主题中的简单示例，更符合你的需要。 可从中了解如何自定义 Nano Server 映像以使其具有所需要功能、从 VHD 或 WIM 安装 Nano Server 映像、编辑文件、处理域、通过多种方法处理包以及处理服务器角色。

## Nano Server 映像生成器

Nano Server 映像生成器是借助图形界面帮助创建自定义 Nano Server 映像和可启动的 USB 媒体的工具。 基于你提供的输入，该生成器将生成可重复使用的 PowerShell 脚本，使你能够轻松实现自动一致安装运行 Windows Server 2016 Datacenter 版本或 Standard 版本的 Nano Server。

从 [下载中心](https://www.microsoft.com/en-us/download/details.aspx?id=54065) 获取工具。 

该工具也需要 [Windows 评估和部署工具包 (ADK)](https://developer.microsoft.com/en-us/windows/hardware/windows-assessment-deployment-kit)。


Nano Server 映像生成器创建 VHD、VHDX 或 ISO格式的自定义 Nano Server 映像，并可创建可启动的 USB 媒体以部署 Nano Server 或检测服务器的硬件配置。 该生成器还可执行以下操作：

- 接受许可条款 
- 创建 VHD、VHDX 或 ISO 格式
- 添加服务器角色
- 添加设备驱动程序
- 设置计算机名、管理员密码、日志文件路径和时区
- 使用现有 Active Directory 帐户或获取的加入域的 Blob 加入域
- 启用 WinRM 进行本地子网外的通信
- 启用虚拟 LAN ID 和配置静态 IP 地址
- 在系统运行时注入新服务包
- 添加要在处理 unattend.xml 后运行的 setupcomplete.cmd 或其他客户脚本
- 为串行端口控制台访问启用紧急管理服务 (EMS)
- 启用开发服务，以启用已经过测试签名的驱动程序和未签名的应用程序，PowerShell 默认 shell
- 启用调试（通过串行、USB、TCP/IP 或 IEEE 1394 协议）
- 创建使用 WinPE 的 USB 媒体，其将对服务器进行分区并安装 Nano 映像
- 创建使用 WinPE 的 USB 媒体，其将检测现有 Nano Server 硬件配置并通过日志或在屏幕上报告所有详细信息。 其包括网络适配器、MAC 地址和固件类型（BIOS 或 UEFI）。 检测过程还将列出系统上的所有卷和不具有包含在服务器核心驱动程序包中的驱动程序的设备。

如果对以上任何内容不熟悉，请查看本主题的其余部分和其他 Nano Server 主题，以便准备好向工具提供其所需的信息。

## <a name="BKMK_CreateImage"></a>创建自定义 Nano Server 映像  
对于 Windows Server 2016，Nano Server 分布在物理介质上，可在其中找到 **NanoServer** 文件夹；其中包含一个 .wim 映像和一个名为 **Packages** 的子文件夹。 这些包文件是你用于将服务器角色和功能添加到 VHD 映像的文件，之后将启动到该映像。  
  
你还可以查找和使用 PackageManagement (OneGet) PowerShell 模块的 NanoServerPackage 提供程序安装这些包。 请参阅本主题的“联机安装角色和功能”部分。  
  
此表显示了此版本的 Nano Server 中可用的角色和功能，以及将为其安装包的 Windows PowerShell 选项。 某些程序包可直接使用其自身的 Windows PowerShell 开关进行安装（例如 -Compute）；其他程序包需通过将其名称传递到 -Package 参数进行安装（可在由逗号分隔的列表中进行组合）。 可使用 Get-NanoServerPackage cmdlet 动态列出可用程序包。  
  
|角色或功能|选项|
|-------------------|----------|
|Hyper-V 角色（包括 NetQoS）|-Compute|
|故障转移群集和其他组件，在此表后详细说明|-Clustering|
|用于各种网络适配器和存储控制器的基本驱动程序。 这是包含在 Windows Server 2016 的服务器核心安装中的同一组驱动程序。|-OEMDrivers|
|文件服务器角色和其他存储组件，在此表后详细说明|-Storage|
|Windows Defender，包括默认签名文件|-Defender|
|用于实现应用程序兼容性的反向转发器，例如常见应用程序框架（如 Ruby、Node.js 等）。|现在默认包括在内|
|DNS 服务器角色|-Package Microsoft-NanoServer-DNS-Package|
|PowerShell Desired State Configuration (DSC)|-Package Microsoft-NanoServer-DSC-Package<br />**注意：** 有关完整的详细信息，请参阅 [使用 Nano Server 上的 DSC](https://msdn.microsoft.com/powershell/dsc/nanoDsc)。|
|Internet 信息服务器 (IIS)|-Package Microsoft-NanoServer-IIS-Package<br />**注意：** 有关使用 IIS 的详细信息，请参阅 [Nano Server 上的 IIS](IIS-on-Nano-Server.md)。|
|针对 Windows 容器的主机支持|-Containers|
|System Center Virtual Machine Manager 代理|-Package Microsoft-NanoServer-SCVMM-Package<br />-Package Microsoft-NanoServer-SCVMM-Compute-Package<br />**注意：** 仅在监视 Hyper-V 时，使用 SCVMM 计算程序包。 对于 VMM 中的超聚合部署，你还应指定 -Storage 参数。 有关更多详细信息，请参阅 [VMM 文档](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-compute-add-nano-hyper-v)。| 
|System Center Operations Manager 代理| 单独安装。 在 System Center Operations Manager 文档中的针对在更多详细信息，请参阅https://technet.microsoft.com/system-center-docs/om/manage/install-agent-on-nano-server。|
|数据中心桥接（包括 DCBQoS）|-Package Microsoft-NanoServer-DCB-Package|
|在虚拟机上部署|-Package Microsoft-NanoServer-Guest-Package|
|在物理计算机上部署|- Package Microsoft-NanoServer-Host-Package|
|BitLocker、受信任的平台模块 (TPM)、卷加密、平台标识、加密提供程序以及与安全启动相关的其他功能|-Package Microsoft-NanoServer-SecureStartup-Package|
|对受防护的 VM 的 Hyper-V 支持|-Package Microsoft-NanoServer-ShieldedVM-Package<br />**注意：** 该程序包仅适用于 Nano Server 的 Datacenter 版本。|
|简单网络管理协议 (SNMP) 代理|-Package Microsoft-NanoServer-SNMP-Agent-Package.cab<br />**注意：** Windows Server 2016 安装媒体中未附带。 仅联机可用。 有关详细信息，请参阅[联机安装角色和功能](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online)。|
|使用 IPv6 转换技术（6to4、ISATAP、端口代理和 Teredo）和 IP-HTTPS 提供隧道连接的 IPHelper 服务|-Package Microsoft-NanoServer-IPHelper-Service-Package.cab<br />**注意：** Windows Server 2016 安装媒体中未附带。 仅联机可用。 有关详细信息，请参阅[联机安装角色和功能](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online)。|

> [!NOTE]  
> 使用这些选项安装程序包时，同时会基于所选服务器媒体区域设置安装相应的语言包。 可在以映像的区域设置命名的子文件夹中的安装媒体中找到可用语言包及其区域设置缩写。  
  
> [!NOTE]  
> 使用 -Storage 参数安装文件服务时，不会实际启用文件服务。 可使用服务器管理器从远程计算机启用此功能。 

### 由 -Clustering 参数安装的故障转移群集项

- 故障转移群集角色
- VM 故障转移群集
- 存储空间直通 (S2D)
- 存储服务质量
- 卷复制群集
- SMB 见证服务


### 由 -Storage 参数安装的文件和存储项

- 文件服务器角色
- 重复数据删除
- 多路径 I/O，包括用于 Microsoft 设备特定模块 (MSDSM) 的驱动程序
- ReFS（v1 和 v2）
- iSCSI 发起程序（但不是 iSCSI 目标）
- 存储副本
- 具有 SMI-S 支持的存储管理服务
- SMB 见证服务
- 动态卷
- 基本 Windows 存储提供程序（用于 Windows 存储管理）
 
  
  
  
### 安装 Nano Server VHD  
本示例演示创建具有给定计算机名称的基于 GPT 的 VHDX 映像，包括 Hyper-V 来宾驱动程序，从网络共享上的 Nano Server 安装媒体开始。 在提升的 Windows PowerShell 提示符下，从此 cmdlet 开始：  
  
`Import-Module <Server media location>\NanoServer\NanoServerImageGenerator; New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\server_en-us -BasePath .\Base -TargetPath .\FirstStepsNano.vhdx -ComputerName FirstStepsNano`  
  
该 cmdlet 将完成以下所有任务：  
  
1.  选择 Standard 作为基本版本  
  
2.  提示输入管理员密码  
  
3.  从 \\\Path\To\Media\server_en-us into .\Base 复制安装媒体  
  
4.  将 WIM 映像转换为 VHD。 （目标路径参数的文件扩展名可确定它创建的是适用于第 1 代虚拟机的基于 MBR 的 VHD 还是适用于第 2 代虚拟机的基于 GPT 的 VHDX。）  
  
5.  将生成的 VHD 复制到 \FirstStepsNano.vhdx  
  
6.  按所指定的那样设置映像的管理员密码  
  
7.  将映像的计算机名设置为 FirstStepsNano  
  
8.  安装 Hyper-V 来宾驱动程序  
  
以上所有操作最后将生成 .\FirstStepsNano.vhdx 映像。  
  
该 cmdlet 将在运行时生成日志并在完成后告知日志的位置。 由配套脚本完成的 WIM-to-VHD 转换会在 %TEMP%\Convert-WindowsImage\\\<GUID> 中生成自己的日志（<GUID> 为每个转换会话的唯一标识符）。  
  
只要使用同一基本路径，就可在每次运行该 cmdlet 时省略媒体路径参数，因为其将使用基本路径中的缓存文件。 如果未指定基本路径，该 cmdlet 将在 TEMP 文件夹中生成默认基本路径。 但如果要使用不同的源媒体，但使用相同的基本路径，则应指定媒体路径参数。  
  
>[!NOTE]  
>现可选择指定 Nano Server 版本以生成 Standard 或 Datacenter 版本。 使用 -Edition 参数可指定 *Standard* 或 *Datacenter* 版本。  
  
具有现有映像后，可使用 *Edit-NanoServerImage* cmdlet 对其进行修改（如需要）。  
  
如果不指定计算机名，将生成随机名称。  
  
### 安装 Nano Server WIM  
  
1.  将 *NanoServerImageGenerator* 文件夹从 Windows Server 2016 ISO 中的 \NanoServer 文件夹复制到计算机上的本地文件夹。  
2. 以管理员身份启动 Windows PowerShell，将目录更改为 NanoServerImageGenerator 文件夹所在的文件夹，然后使用 `Import-Module .\NanoServerImageGenerator -Verbose` 导入模块。  
  
 >[!NOTE]  
>可能必须调整 Windows PowerShell 执行策略。 `Set-ExecutionPolicy RemoteSigned` 应能正常工作。  
  
若要创建 Nano Server 映像以用作 Hyper-V 主机，请运行以下命令：  
  
`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.wim -ComputerName <computer name> -OEMDrivers -Compute -Clustering`  
  
位置  
-   -MediaPath 是 DVD 媒体或包含 Windows Server 2016 的 ISO 映像的根路径。  
-   -BasePath 将包含 Nano Server 二进制文件的副本，因此可使用 New-NanoServerImage -BasePath，且以后运行时无需指定 -MediaPath。  
-   -TargetPath 将包含生成的 .wim 文件，该文件包含所选角色和功能。 请务必指定 .wim 扩展名。  
-   -Compute 将添加 Hyper-V 角色。  
-   -OemDrivers 将添加大量常见驱动程序。  
  
系统将提示输入管理员密码。  
  
有关详细信息，请运行 `Get-Help New-NanoServerImage -Full`。  
   
启动到 WinPE，并确保刚创建的 .wim 文件可从 WinPE 访问。 （例如，可将 wim 文件复制到 USB 闪存驱动器上可启动的 WinPE 映像。）  
  
WinPE 启动后，使用 Diskpart.exe 准备目标计算机的硬盘驱动器。 运行以下 Diskpart 命令（如果使用的不是 UEFI 和 GPT，可相应进行修改）：  
   
> [!WARNING]  
> 这些命令将删除硬盘驱动器上的所有数据。  
  
**Diskpart.exe  
Select disk 0  
clean  
Convert GPT  
Create partition efi size=100  
Format quick FS=FAT32 label="System"  
Assign letter="s"  
Create partition msr size=128  
Create partition primary  
Format quick FS=NTFS label="NanoServer"  
Assign letter="n"  
List volume  
退出**  
   
应用 Nano Server 映像（调整 .wim 文件的路径）：  
  
**Dism.exe /apply-image /imagefile:.\NanoServer.wim /index:1 /applydir:n:\   
Bcdboot.exe n:\Windows /s s:**  
   
删除 DVD 媒体或 USB 驱动器并使用 **Wpeutil.exe Reboot** 重新启动系统  
  
  
### 在本地和以远程方式编辑 Nano Server 上的文件  
 在任一情况下，可使用 Windows PowerShell 远程处理等连接到 Nano Server。  
   
 连接到 Nano Server 后，可通过将文件的相对和绝对路径传递到 psEdit 命令编辑驻留在本地计算机上的文件，例如：   
`psEdit C:\Windows\Logs\DISM\dism.log` 、 `psEdit .\myScript.ps1`  
  
通过使用 `Enter-PSSession -ComputerName "192.168.0.100" -Credential ~\Administrator` 启动远程会话，然后将文件的相对和绝对路径传递到 psEdit 命令，编辑驻留在远程 Nano Server 上的文件，如下所示：   
`psEdit C:\Windows\Logs\DISM\dism.log`  
  
## <a name="BKMK_online"></a>在线安装角色和功能  
> [!NOTE]
> 如果从媒体或联机存储库安装可选 Nano Server 程序包，这将不包括最近的安全修补程序。 为避免可选程序包和基本操作系统之间的版本不匹配，应在安装任意可选程序包后立即安装[最新累积更新](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server)，**然后**再重启服务器。

### 从程序包存储库安装角色和功能  
可使用 PackageManagement PowerShell 模块的 NanoServerPackage 提供程序，从联机程序包存储库中查找和安装 Nano Server 程序包。 若要安装此提供程序，请使用以下 cmdlet：

```powershell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
```

>[!NOTE]
>如果你在运行 Install-PackageProvider 时遇到了错误，请检查你是否已安装了[最新累积更新](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server)（[KB3206632](https://support.microsoft.com/kb/3206632) 或更高版本），或者按以下方式使用 Save-Module： 

```powershell
Save-Module -Path "$Env:ProgramFiles\WindowsPowerShell\Modules\" -Name NanoServerPackage -MinimumVersion 1.0.1.0
Import-PackageProvider NanoServerPackage
```

安装并导入此提供程序后，可通过为使用 Nano Server 程序包专门设计的 cmdlet 搜索、下载和安装 Nano Server 程序包：

```powershell
Find-NanoServerPackage  
Save-NanoServerPackage  
Install-NanoServerPackage
```  
  
你还可使用通用 PackageManagement cmdlet 并且指定 NanoServerPackage 提供程序：

```powershell
Find-Package -ProviderName NanoServerPackage
Save-Package -ProviderName NanoServerPackage
Install-Package -ProviderName NanoServerPackage
Get-Package -ProviderName NanoServerPackage
```
  
若要将上述任何 cmdlet 与 Nano Server 上的 Nano Server 程序包结合使用，请添加 `-ProviderName NanoServerPackage`。 如果不添加 -ProviderName 参数，PackageManagement 将循环访问所有提供程序。 有关上述 cmdlet 的详细信息，请运行 `Get-Help <cmdlet>`。 下面提供了一些常用示例：
    
### 搜索 Nano Server 程序包  
可使用 `Find-NanoServerPackage` 或 `Find-Package -ProviderName NanoServerPackage` 搜索并返回联机存储库中可用的 Nano Server 程序包的列表。 例如，可获取所有最新程序包列表：

```powershell
Find-NanoServerPackage
```
  
运行 `Find-Package -ProviderName NanoServerPackage -DisplayCulture` 会显示所有可用的区域性。

若需要特定区域设置版本，如美国英语，则可使用 `Find-NanoServerPackage -Culture en-us` 或  
`Find-Package -ProviderName NanoServerPackage -Culture en-us` 或 `Find-Package -Culture en-us -DisplayCulture`。

若要根据程序包名查找特定程序包，请使用 -Name 参数。 此参数还接受通配符。 例如，若要查找名称中包含 VMM 的所有程序包，请使用 `Find-NanoServerPackage -Name *VMM*` 或 `Find-Package -ProviderName NanoServerPackage -Name *VMM*`。

可使用 -RequiredVersion、-MinimumVersion 或 -MaximumVersion 参数查找特定版本。 若要查找所有可用版本，请使用 -AllVersions。 否则，仅返回最新版本。 例如：`Find-NanoServerPackage -Name *VMM* -RequiredVersion 10.0.14393.0`。 或者，针对所有版本： `Find-Package -ProviderName NanoServerPackage -Name *VMM* -AllVersions`

### 安装 Nano Server 程序包  
可在本地或通过 `Install-NanoServerPackage` 或 `Install-Package -ProviderName NanoServerPackage` 使用脱机映像将 Nano Server 程序包（包括其依赖项包，若有）安装到 Nano Server。 这两者都接受来自管道的输入。

若要将 Nano Server 程序包的最新版本安装到联机 Nano Server，请使用 `Install-NanoServerPackage -Name Microsoft-NanoServer-Containers-Package` 或 `Install-Package -Name Microsoft-NanoServer-Containers-Package`。 PackageManagement 将使用 Nano Server 的区域性。

可将 Nano Server 程序包安装到脱机映像，同时指定特定版本和区域性，如下所示：

`Install-NanoServerPackage -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

或者：

`Install-Package -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

以下是通过管道将程序包搜索结果传送到安装 cmdlet 的一些示例：  

`Find-NanoServerPackage *dcb* | Install-NanoServerPackage` 查找所有名称中包含“dcb”的程序包，然后安装它们。

`Find-Package *nanoserver-compute-* | Install-Package` 查找名称中包含“nanoserver-compute-”的程序包，然后安装它们。
  
`Find-NanoServerPackage -Name *nanoserver-compute* | Install-NanoServerPackage -ToVhd C:\MyNanoVhd.vhd` 查找名称中包含“compute”的程序包，然后将其安装到脱机映像。

`Find-Package -ProviderName NanoserverPackage *nanoserver-compute-* | Install-Package -ToVhd C:\MyNanoVhd.vhd` 对所有名称中包含“nanoserver-compute-”的程序包执行相同操作。

### 下载 Nano Server 程序包  

`Save-NanoServerPackage` 或 `Save-Package` 允许你下载并保存程序包，而无需安装它们。 这两个 cmdlet 都接受来自管道的输入。

例如，若要下载某一 Nano Server 程序包并将其保存到与通配符路径相匹配的目录中，可使用 `Save-NanoServerPackage -Name Microsoft-NanoServer-DNS-Package -Path C:\`。在本示例中，未指定 -Culture，因此将使用本地计算机的区域性。 未指定版本，因此将保存最新版本。

`Save-Package -ProviderName NanoServerPackage -Name Microsoft-NanoServer-IIS-Package -Path C:\ -Culture it-IT -MinimumVersion 10.0.14393.0` 保存特定版本并针对意大利语和区域设置进行保存。

可通过管道发送搜索结果，如以下示例所示：

`Find-NanoServerPackage -Name *containers* -MaximumVersion 10.2 -MinimumVersion 1.0 -Culture es-ES | Save-NanoServerPackage -Path C:\`

或者

`Find-Package -ProviderName NanoServerPackage -Name *shield* -Culture es-ES | Save-Package -Path`

### 已安装清单的程序包
可发现使用 `Get-Package` 安装的 Nano Server 程序包。 例如，使用 `Get-Package -ProviderName NanoserverPackage` 查看 Nano Server 上的程序包。

若要查看脱机映像中安装的 Nano Server 程序包，请运行 `Get-Package -ProviderName NanoserverPackage -FromVhd C:\MyNanoVhd.vhd`。


### 从本地源安装角色和功能  
虽然建议脱机安装服务器角色和其他程序包，但可能需要在容器方案中联机对其进行安装（运行 Nano Server 时）。 要实现这一点，请执行下列操作：  
  
1.  将程序包文件夹从安装媒体本地复制到运行的 Nano Server（例如：到 C:\packages）。  
  
2.  在另一计算机上创建新的 Unattend.xml 文件，然后将其复制到 Nano Server。 可将此 XML 内容复制并粘贴到创建的 XML 文件（本示例显示如何安装 IIS 包）：  
  
   
     
```  
<?xml version="1.0" encoding="utf-8"?>
    <unattend xmlns="urn:schemas-microsoft-com:unattend">  
    <servicing>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Feature-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" />  
            <source location="c:\packages\Microsoft-NanoServer-IIS-Package.cab" />  
        </package>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Feature-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="en-US" />  
            <source location="c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab" />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source="" xmlns:cpi="urn:schemas-microsoft-com:cpi" />  
</unattend>  
```  
  
  
  
  
3.  在创建（或复制）的新 XML 文件中，将 C:\packages 编辑为程序包内容复制到的目录。  
  
4.  使用新创建的 XML 文件切换到目录并运行  
  
    **dism /online /apply-unattend:.\unattend.xml**  
  
     
  
5.  请确定程序包及其关联的语言包已正确安装，方法是运行：  
  
    **dism /online /get-packages**  
  
    你应该会看到“包标识：Microsoft-NanoServer-IIS-Package~31bf3856ad364e35~amd64~en-US~10.0.10586.0”两次列出，一次用于发布类型：语言包，一次用于发布类型：功能包。  
  
## 自定义现有 Nano Server VHD  
可使用 Edit-NanoServerImage cmdlet 更改现有 VHD 的详细信息，如本示例所示：  
  
`Edit-NanoServerImage   -BasePath .\Base   -TargetPath .\BYOVHD.vhd`  
  
此 cmdlet 与 New-NanoServerImage 执行相同的操作，但会更改现有映像，而不是将 WIM 转换为 VHD。 其支持的参数与 New-NanoServerImage 相同（-MediaPath 和 -MaxSize 除外），因此必须已先使用这些参数创建初始 VHD，才可使用 Edit-NanoServerImage 做出更改。  

## 可使用 New-NanoServerImage 和 Edit-NanoServerImage 完成的其他任务  
  
### 加入域  
New-NanoServerImage 提供加入域的两种方法；两者都依赖于脱机域设置，但其中一个方法获取 Blob 以完成加入。 在本示例中，该 cmdlet 从本地计算机（当然必须为 Contoso 域的一部分）获取 Contoso 域的域 Blob，然后它会使用此 Blob 对映像执行脱机设置：  
  
`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomHarvest.vhdx -ComputerName JoinDomHarvest -DomainName Contoso`  
  
当此 cmdlet 完成时，应会在 Active Directory 计算机列表中找到名为“JoinDomHarvest”的计算机。  
  
还可在未加入域的计算机上使用此 cmdlet。 若要执行此操作，从任何加入域的计算机获取 Blob，然后自行将此 Blob 提供给 cmdlet。 请注意，当从另一计算机获取这种 Blob 时，该 Blob 已包含该计算机名，因此如果尝试添加 *-ComputerName* 参数，将会发生错误。  
  
可使用此命令获取 Blob：  
  
**djoin**  
 **/Provision**  
 **/Domain Contoso**  
 **/Machine JoiningDomainsNoHarvest**  
 **/SaveFile JoiningDomainsNoHarvest.djoin**  
  
使用获取的 Blob 运行 New-NanoServerImage：  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomNoHrvest.vhd -DomainBlobPath .\Path\To\Domain\Blob\JoinDomNoHrvestContoso.djoin`  
  
如果域中已具有与将来的 Nano Server 具有同一计算机名的节点，可通过添加 `-ReuseDomainNode` 参数重新使用该计算机名。  

### 添加其他驱动程序
Nano Server 提供包括一组用于各种网络适配器和存储控制器的基本驱动器的包；其中可能不包括用于你的网络适配器的驱动器。 可使用这些步骤查找工作系统中的驱动程序，将其提取出来，然后将其添加到 Nano Server 映像。

1.  在将要运行 Nano Server 的物理计算机上安装 Windows Server 2016。
2.  打开设备管理器并识别以下类别的设备：
* 网络适配器
* 存储控制器
* 磁盘驱动器
3.  对于以上类别中的每个设备，请右键单击设备名称，然后单击**属性**。 在打开的对话框中，单击**驱动程序**选项卡，然后单击**驱动程序详细信息**。
4.  记下显示的驱动程序文件的文件名和路径。 例如，假设驱动程序文件是 e1i63x64.sys（位于 C:\Windows\System32\Drivers）。
5.  在命令提示符中，搜索驱动程序文件并搜索带有 e1i*.sys /s /b 的所有实例。 在本示例中，驱动程序文件也存在于路径 C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd\e1i63x64.sys 中。
6.  在提升的命令提示符中，导航到 Nano Server VHD 所在目录并运行以下命令：**md mountdir**
     
     **dism\dism /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**
     
     **dism\dism /Add-Driver /image:.\mountdir /driver: C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd**
     
     **dism\dism /Unmount-Image /MountDir:.\MountDir /Commit**
7.  为每个所需的驱动程序文件重复以上步骤。

> [!NOTE]  
> 保存驱动程序的文件夹中必须存在 SYS 文件和相应的 INF 文件。 而且，Nano Server 仅支持已签名的 64 位驱动程序。 
  
### 注入驱动程序  
Nano Server 提供包括一组用于各种网络适配器和存储控制器的基本驱动器的包；其中可能不包括用于你的网络适配器的驱动器。 可使用此语法让 New-NanoServerImage 搜索目录以寻找可用驱动程序，并将其注入到 Nano Server 映像：  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers`  
  
> [!NOTE]
> 保存驱动程序的文件夹中必须存在 SYS 文件和相应的 INF 文件。 而且，Nano Server 仅支持已签名的、64 位驱动程序。

使用 -DriverPath 参数，你还可以将路径数组传递到驱动程序 .inf 文件：

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers\netcard64.inf`

### 使用 WinRM 进行连接  
若要能够使用 Windows 远程管理 (WinRM) 来连接到 Nano Server 计算机（从并非位于同一子网的另一计算机），请打开端口 5985 以实现 Nano Server 映像上的入站 TCP 流量。 使用此 cmdlet：  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\ConnectingOverWinRM.vhd -EnableRemoteManagementPort`  
  
### 设置静态 IP 地址  
若要将 Nano Server 映像配置为使用静态 IP 地址，请首先通过 Get-NetAdapter、netsh 或 Nano Server 恢复控制台查找要修改的接口的名称或索引。 使用 -Ipv6Address、-Ipv6Dns、-Ipv4Address、-Ipv4SubnetMask、-Ipv4Gateway 和 -Ipv4Dns parameters 参数指定配置，如本示例中所示：  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\StaticIpv4.vhd -InterfaceNameOrIndex Ethernet -Ipv4Address 192.168.1.2 -Ipv4SubnetMask 255.255.255.0 -Ipv4Gateway 192.168.1.1 -Ipv4Dns 192.168.1.1`  
  
### 自定义映像大小  
可使用 -MaxSize 参数将 Nano Server 映像配置为动态扩展的 VHD 或 VHDX，如本例所示：  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -MaxSize 100GB`  
  
### 嵌入自定义数据  
若要在 Nano Server 映像中嵌入自己的脚本或二进制文件，请使用 -CopyPath 参数来传递要复制的文件和目录数组。 该 -CopyPath 参数还可以接受哈希表，以便指定文件和目录的目标路径。

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -CopyPath .\tools`

### 第一次启动后运行自定义命令
若要运行自定义命令作为 setupcomplete.cmd 的一部分，请使用 -SetupCompleteCommand 参数来传递命令数组：

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -SetupCompleteCommand @("echo foo", "echo bar")`


### 运行自定义 PowerShell 脚本作为映像创建的一部分
若要运行自定义 PowerShell 脚本作为映像创建流程的一部分，请使用 -OfflineScriptPath 参数将路径数组传递到 .ps1 脚本。 如果这些脚本带实参，则使用 -OfflineScriptArgument 将附加实参的哈希表传递到这些脚本。

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -OfflineScriptPath C:\MyScripts\custom.ps1 -OfflineScriptArgument @{Param1="Value1"; Param2="Value2"}`


### 对开发方案的支持
若要在 Nano Server 上开发和测试，可使用 -Development 参数。 这将允许 PowerShell 作为默认本地 shell、允许未签名驱动程序的安装、复制调试程序二进制文件、打开调试端口、启用测试签名，以及允许无需开发人员许可证即可安装 AppX 程序包：

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`


### 自定义无人参与文件  
若要使用自己的无人参与文件，可使用 -UnattendPath 参数：  
  
`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -UnattendPath \\path\to\unattend.xml`  
  
在此无人参与文件中指定管理员密码或计算机名将覆盖由 -AdministratorPassword 和 -ComputerName 设置的值。 

> [!NOTE]
> Nano Server 不支持通过无人参与文件设置 TCP/IP 设置。 你可以使用 Setupcomplete.cmd 配置 TCP/IP 设置。

### 收集日志文件
如果你想要在映像创建过程中收集日志文件，请使用 -LogPath 参数指定所有日志文件复制到的目录。

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -LogPath C:\Logs`


> [!NOTE]
> New-NanoServerImage 和 Edit-NanoServerImage 上的某些参数仅供内部使用，可以放心地忽略。 这些参数包括 -SetupUI 和 -Internal 参数。


## 安装应用和驱动程序
[comment]: # (从 Xumin 阳光 bug #68620。)  

### Windows Server 应用安装程序
Windows Server 应用 (WSA) 安装程序为 Nano Server 提供可靠的安装选项。 由于 Nano Server 上不支持 Windows Installer (MSI)，所以 WSA 也是可用于非 Microsoft 产品的唯一安装技术。 WSA 利用 Windows 应用包技术，该技术旨在使用声明性清单安全可靠地安装和服务于应用程序。 该应用将扩展 Windows 应用包安装程序以支持特定于 Windows Server 的扩展，但 WSA 不支持安装驱动程序。

在 Nano Server 上创建和安装 WSA 包同时涉及该包的发布者和使用者所需执行的操作。

该程序包发布者应执行以下操作：

1. 安装 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)，包括创建 WSA 包所需的工具：MakeAppx、MakeCert、Pvk2Pfx、SignTool。
2. 宣布清单：按照 [WSA 清单扩展架构](https://msdn.microsoft.com/library/windows/apps/mt670653.aspx)来创建清单文件，AppxManifest.xml。
3. 使用 **MakeAppx** 工具创建 WSA 包。
4. 使用 **MakeCert** 和 **Pvk2Pfx** 工具创建证书，然后使用 **Signtool** 对包进行签名。

接下来，包使用者应执行以下步骤：

1. 运行 [*Import-Certificate*](https://technet.microsoft.com/library/hh848630) PowerShell cmdlet，使用位于“Cert:\LocalMachine\TrustedPeople”的 certStoreLocation 将上面步骤 4 中的发布者证书导入到 Nano Server。 例如： `Import-Certificate -FilePath ".\xyz.cer" -CertStoreLocation "Cert:\LocalMachine\TrustedPeople"`
2. 通过运行 [**Add-AppxPackage**](https://technet.microsoft.com/library/mt575516(v=wps.620).aspx) PowerShell cmdlet 在 Nano Server 上安装 WSA 包，从而将该应用安装在 Nano Server 上。 例如： `Add-AppxPackage wsaSample.appx`

#### 用于创建应用的其他资源
WSA 是 Windows 应用包技术的服务器扩展（尽管该应用未在 Microsoft Store 中托管）。 若要使用 WSA 发布应用，以下主题可帮助你了解应用包管道：

- [如何创建基本的程序包清单](https://msdn.microsoft.com/library/windows/desktop/br211475.aspx)
- [应用包生成工具 (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)
- [如何创建应用包签名证书](https://msdn.microsoft.com/library/windows/desktop/jj835832(v=vs.85).aspx)
- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)

### 在 Nano Server 上安装驱动程序
可使用 INF 驱动程序包在 Nano Server 上安装非 Microsoft 驱动程序。 其中包括即插即用 (PnP) 驱动程序包和文件系统筛选器驱动程序包。 当前，Nano Server 不支持网络筛选器驱动程序。

PnP 和文件系统筛选器驱动程序包必须遵循通用驱动程序要求和安装流程以及常规驱动程序包指南（如签名）。 它们记录在以下位置：

- [驱动程序签名](https://msdn.microsoft.com/windows/hardware/drivers/install/driver-signing)
- [使用通用 INF 文件](https://msdn.microsoft.com/windows/hardware/drivers/install/using-a-configurable-inf-file)

#### 脱机安装驱动程序包

可通过 [DISM.exe](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/dism-driver-servicing-command-line-options-s14) 或 [DISM PowerShell](https://technet.microsoft.com/library/dn376497.aspx) cmdlet 将支持的驱动程序包脱机安装到 Nano Server。

#### 联机安装驱动程序包
可使用 [PnpUtil](https://msdn.microsoft.com/library/windows/hardware/ff550419(v=vs.85).aspx) 将 PnP 驱动程序包联机安装到 Nano Server。 Nano Server 当前不支持非 PnP 驱动程序包的联机驱动程序安装。





 
--------------------------------------------------  
  
  
## <a name="BKMK_JoinDomain"></a>将 Nano Server 加入域  
  
### 将 Nano Server 联机添加到域  
  
1.  使用以下命令从域中已在运行 Windows Threshold Server 的计算机获取数据 Blob：  
  
    `djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  
  
    这会将数据 Blob 保存到名为“odjblob”的文件中。  
  
2.  使用以下命令将“odjblob”文件复制到 Nano Server 计算机：  
  
    **net use z: \\\\\<ip address of Nano Server>\c$**  
  
    > [!NOTE]  
    > 如果 net use 命令失败，可能需要调整 Windows 防火墙规则。 要执行此操作，首先打开提升的命令提示符，启动 Windows PowerShell，然后使用以下命令通过 Windows PowerShell 远程处理连接到 Nano Server 计算机：  
    >   
    > `Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
    >   
    > `$ip = "<ip address of Nano Server>"`  
    >   
    > `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  
    >   
    > 出现提示时，提供管理员密码，然后运行以下命令以设置防火墙规则：  
    >   
    > **netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes**  
    >   
    > 使用 `Exit-PSSession` 退出 Windows PowerShell，然后重试 net use 命令。 如果成功，则继续将“odjblob”文件内容复制到 Nano Server。  
  
    **md z:\Temp**  
  
    **复制 odjblob z:\Temp**  
  
3.  检查想要 Nano Server 加入的域以确保已配置 DNS。 此外，还需验证域的名称解析或域控制器是否按预期工作。 要执行此操作，打开提升的命令提示符，启动 Windows PowerShell，然后使用以下命令通过 Windows PowerShell 远程处理连接到 Nano Server 计算机：  
  
    `Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
    `$ip = "<ip address of Nano Server>"`  
  
    `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  
  
    出现提示时，提供管理员密码。 Nslookup 在 Nano Server 上不可用，可使用 Resolve-DNSName 验证名称解析。

4. 如果名称解析成功，则在同一 Windows PowerShell 会话中运行此命令以加入域：  
  
    `djoin /requestodj /loadfile c:\Temp\odjblob /windowspath c:\windows /localos`  
  
5.  重新启动 Nano Server 计算机，然后退出 Windows PowerShell 会话：  
  
    `shutdown /r /t 5`  
  
    `Exit-PSSession`  
  
6.  将 Nano Server 加入到域后，将域用户帐户添加到 Nano Server 上的管理员组。

7. 为安全起见，请使用以下命令将 Nano Server 从受信任的主机列表中删除：
`Set-Item WSMan:\localhost\client\TrustedHosts ""` 
  
**一步加入域的备用方法**  
  
首先，使用此命令从正在运行 Windows Threshold Server 的另一计算机（已在域中）获取数据 Blob：  
  
`djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  
  
打开文件“odjblob”（可能在记事本中），然后将其内容复制并粘贴到下面的 Unattend.xml 文件的 <AccountData> 部分。  
  
将 unattend.xml 文件放入 C:\NanoServer 文件夹，然后使用以下命令装载 VHD 并在 `offlineServicing` 部分应用设置：  
  
**dism\dism /Mount-ImagemediaFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**  
  
**dism\dismmedia:.\mountdir /Apply-Unattend:.\unattend.xml**  
  
创建“Panther”文件夹（Windows 系统使用该文件夹在安装过程中存储文件；如感兴趣，请参阅 [Windows 7、Windows Server 2008 R2 和 Windows Vista 安装日志文件位置](https://support.microsoft.com/en-us/kb/927521)，将 Unattend.xml 文件复制到该文件夹，然后使用以下命令卸载 VHD：  
  
**md.\mountdir\windows\panther**  
  
**copy .\unattend.xml .\mountdir\windows\panther**  
  
**dism\dism /Unmount-Image /MountDir:.\mountdir /Commit**  
  
第一次从此 VHD 启动 Nano Server 时，将会应用其他设置。  
  
将 Nano Server 加入到域后，将域用户帐户添加到 Nano Server 上的管理员组。  
 
## 使用 Nano Server 上的服务器角色

###使用 Nano Server 上的 Hyper-V  
Hyper-V 在 Nano Server 上的工作方式和在服务器核心模式下的 Windows Server 上的工作方式相同，但有两点不同：  
  
-   必须远程执行所有管理，且管理计算机必须运行与 Nano Server 相同的 Windows Server 版本。 旧版本的 Hyper-V 管理器或 Hyper-V Windows PowerShell cmdlet 将无法工作。  
  
-   RemoteFX 不可用。  
  
在此版本中，已验证 Hyper-V 的以下功能：  
  
-   启用 Hyper-V  
  
-   第 1 代和第 2 代虚拟机的创建  
  
-   虚拟交换机的创建  
  
-   启动虚拟机并运行 Windows 来宾操作系统  
- Hyper-V 副本  
  
  
  
如果要执行虚拟机的实时迁移，则在 SMB 共享上创建虚拟机或将现有 SMB 共享上的资源连接到现有虚拟机，且务必正确配置身份验证。 可通过两种方式执行此操作：  
  
**约束的委派**  
  
约束的委派的工作方式与之前的版本相同。 有关详细信息，请参阅以下文章：  
  
-   [启用 Hyper-V 远程管理 - 为 SMB 和高度可用的 SMB 配置约束的委派](http://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-smb-and-highly-available-smb.aspx)  
  
-   [启用 Hyper-V 远程管理 - 为非群集的实时迁移配置约束的委派](http://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-non-clustered-live-migration.aspx)  
  
**CredSSP**  
  
首先参阅本主题的“使用 Windows PowerShell 远程处理”部分以启用和测试 CredSSP。 然后，可在管理计算机上使用 Hyper-V 管理器，并选择选项“以其他用户身份连接”。 Hyper-V 管理器将使用 CredSSP。 即使使用的是当前帐户，也须执行此操作。  
  
Hyper-V 的 Windows PowerShell cmdlet 可使用 CimSession 或 Credential 参数，两者都可使用 CredSSP。  
  
### <a name="BKMK_Failover"></a>使用 Nano Server 上的故障转移群集  
故障转移群集在 Nano Server 上的工作方式和在服务器核心模式下的 Windows Server 上的工作方式相同，但请记住以下注意事项：  
  
-   必须使用故障转移群集管理器或 Windows PowerShell 远程管理群集。  
  
-   所有 Nano Server 群集节点都必须加入同一域中，类似于 Windows Server 中的群集节点。  
  
-   域帐户必须具有对所有 Nano Server 节点的管理员权限，与 Windows Server 中的群集节点一样。  
  
-   必须在提升的命令提示符中运行所有命令。  
  
> [!NOTE]  
> 此外，此版本不支持某些功能：  
>   
> -   无法通过 Windows PowerShell 在本地 Nano Server 上运行故障转移群集 cmdlet。  
> -   除 Hyper-V 和文件服务器之外的群集角色。  
  
你会发现这些 Windows PowerShell cmdlet 在管理故障转移群集时会很有用：  
  
可创建新群集 `New-Cluster -Name <clustername> -Node <comma-separated cluster node list>`  
  
建立新群集后，应在所有节点上运行 `Set-StorageSetting -NewDiskPolicy OfflineShared`。  
  
将其他节点添加到群集，方法是使用 `Add-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  
  
从群集中删除节点，方法是使用  `Remove-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  
  
创建横向扩展文件服务器，方法是使用 `Add-ClusterScaleoutFileServerRole -name <sofsname> -cluster <clustername>`  
  
可在 [Microsoft.FailoverClusters.PowerShell](https://technet.microsoft.com/library/ee461009.aspx) 找到用于故障转移群集的其他 cmdlet。  
  
### <a name="BKMK_DNS"></a>在 Nano Server 上使用 DNS 服务器  
若要为 Nano Server 提供 DNS 服务器角色，请将 Microsoft-NanoServer-DNS-Package 添加到映像（请参阅本主题的“创建自定义 Nano Server 映像”部分。 Nano Server 开始运行后，请连接它并从提升的 Windows PowerShell 控制台运行此命令以启用该功能：  
  
`Enable-WindowsOptionalFeature -Online -FeatureName DNS-Server-Full-Role`  
  
### <a name="BKMK_IIS"></a>使用 Nano Server 上的 IIS  
有关使用 Internet 信息服务 (IIS) 角色的步骤，请参阅 [Nano Server 上的 IIS](IIS-on-Nano-Server.md)。 

### 使用 Nano Server 上的 MPIO
有关使用 MPIO 的步骤，请参阅 [Nano Server 上的 MPIO](MPIO-on-Nano-Server.md) 

### <a name="BKMK_SSH"></a>使用 Nano Server 上的 SSH
若要了解如何通过 OpenSSH 项目安装和使用 Nano Server 上的 SSH，请参阅 [Win32-OpenSSH wiki](https://github.com/PowerShell/Win32-OpenSSH/wiki)。

## 附录：用于将 Nano Server 加入到域的 Unattend.xml 文件的示例  
  
> [!NOTE]  
> 将“odjblob”粘贴到无人参与文件中后，请务必删除“odjblob”内容中的尾部空格。  
  
```  
<?xml version='1.0' encoding='utf-8'?>  
<unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
  <settings pass="offlineServicing">  
    <component name="Microsoft-Windows-UnattendedJoin" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
        <OfflineIdentification>                
           <Provisioning>    
             <AccountData> AAAAAAARUABLEABLEABAoAAAAAAAMABSUABLEABLEABAwAAAAAAAAABbMAAdYABc8ABYkABLAABbMAAEAAAAMAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABAsAAAAAAAQAAZoABNUABOYABZYAANQABMoAAOEAAMIAAOkAANoAAMAAAXwAAJAAAAYAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABLEAALMABLQABU0AATMABXAAAAAAAKdf/mhfXoAAUAAAQAAAAb8ABLQABbMABcMABb4ABc8ABAIAAAAAAb8ABLQABbMABcMABb4ABc8ABLQABb0ABZIAAGAAAAsAAR4ABTQABUAAAAAAACAAAQwABZMAAZcAAUgABVcAAegAARcABKkABVIAASwAAY4ABbcABW8ABQoAAT0ABN8AAO8ABekAAJMAAVkAAZUABckABXEABJUAAQ8AAJ4AAIsABZMABdoAAOsABIsABKkABQEABUEABIwABKoAAaAABXgABNwAAegAAAkAAAAABAMABLIABdIABc8ABY4AADAAAA4AAZ4ABbQABcAAAAAAACAAkKBW0ID8nJDWYAHnBAXE77j7BAEWEkl+lKB98XC2G0/9+Wd1DJQW4IYAkKBAADhAnKBWEwhiDAAAM2zzDCEAM6IAAAgAAAAAAAQAAAAAAAAAAAABwzzAAA  
</AccountData>  
           </Provisioning>    
         </OfflineIdentification>    
    </component>  
  </settings>  
  
  <settings pass="oobeSystem">  
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
      <UserAccounts>  
        <AdministratorPassword>  
           <Value>Tuva</Value>  
           <PlainText>true</PlainText>  
        </AdministratorPassword>  
      </UserAccounts>  
      <TimeZone>Pacific Standard Time</TimeZone>  
    </component>  
  </settings>  
  
  <settings pass="specialize">  
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
      <RegisteredOwner>My Team</RegisteredOwner>  
      <RegisteredOrganization>My Corporation</RegisteredOrganization>  
    </component>  
  </settings>  
</unattend>  
```  
  
