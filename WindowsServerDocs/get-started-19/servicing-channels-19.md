---
title: 服务频道
description: Windows Server 服务通道的说明：LTSC 和 SAC
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: high
ms.openlocfilehash: c4329339e37acbb0b589e767a570073f4ae1363f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848728"
---
# <a name="windows-server-servicing-channels-ltsc-and-sac"></a>Windows Server 服务通道：LTSC 和 SAC

>适用于：Windows Server 2019、Windows Server 2016


**有两个主要发布通道可供 Windows Server 客户，长期服务频道和半年频道。** 

你可以将服务器置于长期服务频道 (LTSC)、移到新半年频道或将部分服务器置于任一频道，具体取决于你的需求。


## <a name="long-term-servicing-channel-ltsc"></a>长期服务频道 (LTSC)
用户已熟悉了解该版本模型（以前称为“Long-Term Servicing *Branch*”)，其中 Windows Server 的全新主要版本每 2-3 年发布一次。 用户有权享受 5 年的主流支持和 5 年的延长支持。 该渠道适用于需要更长时间服务选项和功能稳定性的系统。 新的半年频道版本不会影响 Windows Server 2016 和 Windows Server 早期版本的部署工作。 长期服务频道将持续接受安全和非安全更新，但不会接受新特性和新功能。

> [!Note]  
> **当前的 LTSC 产品是 Windows Server 2019**。 如果你想坚持使用此频道，则应该安装（或继续使用）Windows Server 2019，你可以通过服务器核心安装选项或带桌面体验的服务器安装选项进行安装。

## <a name="semi-annual-channel"></a>半年频道 
半年频道非常适合将快速创新才能利用新的操作系统功能以更快的速度，同时在应用程序 – 尤其是那些构建在容器和微服务，以及在软件定义的客户混合数据中心。 以半年频道发布的 Windows Server 产品每年发布两次新版本，分别在春季和秋季。 每个以该频道发布的版本自初始版本开始享受 18 个月的支持服务。

半年频道中引入的大部分功能将汇总至 Windows Server 的 Long-Term Servicing Channel 版本。 版本、功能和支持内容可能因版本而异，具体取决于客户反馈。

可使用[软件保障](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx)，也可以通过 Azure Marketplace 或其他云/托管服务提供商及 Visual Studio 订阅等会员计划为批量授权的客户提供半年频道。

> [!Note]  
> **当前的半年频道版本是 Windows Server 版本 1809**。 如果你想将服务器置于此频道，则应该安装 Windows Server 版本 1809，该版本可以在服务器核心模式下安装或作为容器中运行的 Nano Server 来安装。 不支持从 Windows Server 2016 就地升级为 Windows Server 版本 1809，因为两者位于**不同的发布频道**。 Windows Server 版本 1809 不是 Windows Server 2016 的更新版，而是此半年频道上的下一个 Windows Server 版本。



在该模型中，Windows Server 版本通过发布的年份和月份进行标识：例如，2017 年 9 月（九月）发布的版本标识为**版本 1709**。 以半年频道发布的 Windows Server 每年发布两次新版本。 每个版本的支持周期为 18 个月。

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>在 LTSC 上保留服务器还是将它们转移至半年频道？
以下是需考虑的主要区别：

- 是否需要快速创新？ 是否需要提前访问最新的 Windows Server 功能？ 是否需要支持快速节奏混合应用程序、dev-ops 和 Hyper-V 结构？ 如果需要，你应该考虑安装 **Windows Server 版本 1809**，**加入半年频道**。 如本主题中所述，每年你将收到两个新版本，每个版本的主流生产支持期为 18 个月。 可通过批量许可、Azure 或 Visual Studio 订阅服务获取新版本。 目前，你需要批量许可和软件保障才能在生产中运行以半年频道发布的新版本。
- 是否需要稳定性和可预测性？ 是否需要在物理服务器上运行虚拟机和传统工作负载？ 如果需要，你应该考虑**将服务器置于长期服务频道**。 当前的 LTSC 版本为 **Windows Server 2019**。 如本主题中所述，你有权每 2 至 3 年接收新版本，每个版本享受 5 年的主流支持和 5 年的延长支持。 LTSC 版本在所有发布机制中均有提供。 无论使用何种许可模型，所有人均可使用 LTSC 版本。 

下表总结了不同频道之间的主要差异：

|  | 长期服务频道 (Windows Server 2019) |半年频道 (Windows Server) |
| ------------------- | ------------------------------------ | ------------------------------------------------- |
|建议应用场景 | 通用文件服务器、Microsoft 和非 Microsoft 工作负载、传统应用、基础架构角色、软件定义数据中心和超级集成基础架构 | 容器化应用程序、容器主机和受益于更快创新的应用程序方案 |
| 最新发布 | 每 2 - 3 年 |每 6 个月 |
| 支持 |5 年的主流支持和 5 年的延长支持 | 18 个月 |
| 版本 | 所有可用的 Windows Server 版本 | 标准版和数据中心版 |
| 谁可以使用 | 所有频道的所有客户 | 仅软件保障客户和云客户 |
| 安装选项 | Server Core 和带桌面体验的 Server | 容器主机服务器核心、映像和 Nano Server 容器映像 |                |


## <a name="device-compatibility"></a>设备兼容性
除非另行沟通，否则运行半年频道版本的最低硬件要求将与运行最新 Windows Server Long-Term Servicing Channel 版本的要求相同。 例如，**当前 Long-Term Servicing Channel 版本为 Windows Server 2019**。 大多数硬件驱动器仍可在这些版本中正常工作。

## <a name="servicing"></a>维护
Long-Term Servicing Channel 和半年频道两种版本都将由安全更新和非安全更新进行支持。 不同之处在于版本受支持的时间长度，如前文所述。

### <a name="servicing-tools"></a>维护工具
IT 专业人员可以使用多种工具维护 Windows Server。 每个选项均有其优点和缺点，范围从功能和控制到简洁性和管理要求低。 以下是可用于管理维护更新的维护工具示例：

- **Windows 更新 （独立）**:此选项才可用的服务器的已连接到 Internet 并且已启用的 Windows 更新。
- **Windows Server Update Services (WSUS)** 可在大范围内控制 Windows 10 和 Windows Server 更新，并且内置在 Windows Server 操作系统中。 除了能够延迟更新，组织还可以添加用于更新的批准层，并在准备就绪后将它们部署到特定计算机或计算机组。
- **System Center Configuration Manager** 可最大程度地控制维护。 IT 专业人员可以延迟更新、批准更新，并且可以使用多种选项指向部署以及管理带宽使用情况和部署次数。

你可能已基于你的资源、员工和专业知识选择使用此类选项中的至少一项。 你可以继续将相同的流程用于半年频道版本：例如，如果你已使用 System Center Configuration Manager 管理更新，则可以继续使用。 同样，如果你正在使用 WSUS，也可以继续使用。

## <a name="obtain-preview-releases-through-the-windows-insider-program"></a>通过 Windows 预览体验计划获取预览版本
测试较早版本的 Windows Server 对 Microsoft 及其客户都有帮助，因为他们有机会在发布之前发现可能出现的问题。 此外，这还为客户提供了一个直接影响产品功能的独特机会。 

Microsoft 依赖于在开发过程中接收反馈，从而可以尽快做出调整。 提前测试和反馈对于快速发布模型至关重要。

有关如何参与 Windows 预览体验计划的详细信息，请参阅 [Windows 预览体验计划服务器版文档](https://docs.microsoft.com/windows-insider/at-work/)。

## <a name="to-identify-windows-server-2019-and-windows-server-version-1809"></a>区分 Windows Server 2019 和 Windows Server 版本 1809

>[!Note]  
> 以下指南旨在帮助识别并区分 LTSC 和 SAC，且仅用于生命周期和常规清单目的，  而不用于应用程序兼容性或用于表示特定的 API 图面。  应用开发人员应使用其它指南，以在系统生命周期内添加组件、API 和功能之前或之时确保它们的兼容性。 [操作系统版本](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version)对于开发人员来说更好入手。

打开 Powershell，并使用 Get-ItemProperty Cmdlet 或 Get-ComputerInfo Cmdlet 检查注册表中的相应属性。  结合属性和内部版本号，即可确认品牌年度（如 2019 年）中是否存在 LTSC 或 SAC。  LTSC 拥有内部版本号，但 SAC 没有。  该命令还会返回版本发布时的 ReleaseId 或 WindowsVersion（如版本 1809），同时提示本次安装的是服务器核心还是带桌面体验的服务器。 

**Windows Server 2019 数据中心版 (LTSC) 桌面体验的示例：**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server 2019 Datacenter
ReleaseId                 : 1809
InstallationType          : Server
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Windows Server，版本 1809 (SAC) Standard Edition 服务器核心示例：**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server Standard
ReleaseId                 : 1809
InstallationType          : Server Core
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Windows Server 2019 Standard Edition (LTSC) 服务器核心示例：**


````PowerShell
Get-ComputerInfo | Select WindowsProductName, WindowsVersion, WindowsInstallationType, OsServerLevel, OsVersion, OsHardwareAbstractionLayer
````

````
WindowsProductName            : Windows Server 2019 Standard
WindowsVersion                : 1809
WindowsInstallationType       : Server Core
OsServerLevel                 : ServerCore
OsVersion                     : 10.0.17763
OsHardwareAbstractionLayer    : 10.0.17763.107
````

若要查询新的[服务器核心应用程序兼容性 FOD](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) 是否存在于服务器上，请使用 [Get WindowsCapability](https://docs.microsoft.com/powershell/module/dism/get-windowscapability?view=win10-ps) Cmdlet 查找：
````
Name    :     ServerCore.AppCompatibility~~~~0.0.1.0
State   :     Installed
````

# <a name="related-topics"></a>相关主题
[对 Windows Server 半年频道中的 Nano Server 的更改](../get-started/nano-in-semi-annual-channel.md)

[Windows Server 支持生命周期](https://support.microsoft.com/lifecycle)

[确定是否正在运行服务器核心](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[GetProductInfo 函数](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[软件清单日志记录 Cmdlet](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
