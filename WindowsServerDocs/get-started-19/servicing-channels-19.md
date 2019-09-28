---
title: 服务频道
description: Windows Server 服务频道的说明：LTSC 和 SAC
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 05/21/2019
ms.openlocfilehash: 814bcf3e989e9aa9b83ba447d07c45ee95309a5a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391972"
---
# <a name="windows-server-servicing-channels-ltsc-and-sac"></a>Windows Server 服务频道：LTSC 和 SAC

>适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

目前为 Windows Server 客户提供的两种主要版本频道为长期服务频道和半年频道。

可将服务器置于长期服务频道 (LTSC)、移到新半年频道或将部分服务器置于任一频道，具体取决于需求。

## <a name="long-term-servicing-channel-ltsc"></a>长期服务频道 (LTSC)

这是你已熟悉的版本模型（以前称为“Long-Term Servicing *Branch*”），其中 Windows Server 的全新主要版本每 2-3 年发布一次。 用户有权享受 5 年的主流支持和 5 年的延长支持。 该频道适用于需要更长时间服务选项和功能稳定性的系统。 新的半年频道版本不会影响 Windows Server 2016 和 Windows Server 早期版本的部署工作。 长期服务频道将持续接受安全和非安全更新，但不会接受新特性和新功能。

> [!Note]  
> **当前的 LTSC 产品是 Windows Server 2019**。 如果你想坚持使用此频道，则应该安装（或继续使用）Windows Server 2019，你可以通过 Server Core 安装选项或带桌面体验的服务器安装选项进行安装。

## <a name="semi-annual-channel"></a>半年频道

半年频道是快速创新客户的最佳选- 这类用户可以在应用程序（特别是基于容器和微服务的应用程序）以及软件定义混合数据中心中更快地利用新操作系统功能。 以半年频道发布的 Windows Server 产品每年发布两次新版本，分别在春季和秋季。 每个以该频道发布的版本自初始版本开始享受 18 个月的支持服务。

半年频道中引入的大部分功能将汇总至 Windows Server 的 Long-Term Servicing Channel 版本。 版本、功能和支持内容可能因版本而异，具体取决于客户反馈。

可使用[软件保障](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx)，也可以通过 Azure 市场或其他云/托管服务提供商及 Visual Studio 订阅等会员计划为批量授权的客户提供半年频道。

> [!Note]  
> **当前的半年频道版本是 Windows Server 版本 1903**。 如果你要将服务器置于此频道，则应该安装 Windows Server 版本 1903，该版本可以在 Server Core 模式下安装或作为容器中运行的 Nano Server 来安装。 不支持从长期服务频道版本就地升级，因为它们处于**不同的发行频道**中。 半年频道发行版不是更新 – 它是半年频道中的下一个 Windows Server 发行版。

在该模型中，Windows Server 版本通过发布的年份和月份进行标识：例如，2017 年 9 月发布的版本标识为**版本 1709**。 以半年频道发布的 Windows Server 每年发布两次新版本。 每个版本的支持周期为 18 个月。

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>在 LTSC 上保留服务器还是将它们转移至半年频道？

以下是需考虑的主要区别：

- 是否需要快速创新？ 是否需要提前访问最新的 Windows Server 功能？ 是否需要支持快速节奏混合应用程序、dev-ops 和 Hyper-V 结构？ 如果需要，应该考虑安装 **Windows Server 版本 1903**，**加入半年频道**。 如本主题中所述，每年你将收到两个新版本，每个版本的主流生产支持期为 18 个月。 可通过批量许可、Azure 或 Visual Studio 订阅服务获取新版本。 目前，如果你打算在生产环境中运行产品，半年频道中的版本需要批量许可和软件保障。
- 是否需要稳定性和可预测性？ 是否需要在物理服务器上运行虚拟机和传统工作负载？ 如果需要，应该考虑**将服务器置于长期服务频道**。 当前的 LTSC 版本为 **Windows Server 2019**。 如本主题中所述，你有权每 2 至 3 年接收新版本，每个版本享受 5 年的主流支持和 5 年的延长支持。 LTSC 版本在所有发布机制中均有提供。 无论使用何种许可模型，所有人均可使用 LTSC 版本。 

下表总结了不同频道之间的主要差异：


|                       |                                                              长期服务频道 (Windows Server 2019)                                                               |                                   半年频道 (Windows Server)                                   |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| 建议方案 | 通用文件服务器、Microsoft 和非 Microsoft 工作负载、传统应用、基础架构角色、软件定义数据中心和超融合基础设施 | 容器化应用程序、容器主机和受益于更快创新的应用程序方案 |
|     最新发布      |                                                                               每 2 - 3 年                                                                                |                                              每 6 个月                                              |
|        支持        |                                                       5 年的主流支持和 5 年的延长支持                                                        |                                                18 个月                                                 |
|       版本        |                                                                    所有可用的 Windows Server 版本                                                                     |                                     标准版和数据中心版                                     |
|      谁可以使用      |                                                                      所有频道的所有客户                                                                      |                               仅软件保障客户和云客户                                |
| 安装选项  |                                                                Server Core 和带桌面体验的 Server                                                                |                 容器主机 Server Core、映像和 Nano Server 容器映像                 |

## <a name="device-compatibility"></a>设备兼容性

除非另行沟通，否则运行半年频道版本的最低硬件要求将与运行最新 Windows Server 长期服务频道版本的要求相同。 例如，**当前长期服务频道版本为 Windows Server 2019**。 大多数硬件驱动器仍可在这些版本中正常工作。

## <a name="servicing"></a>维护

长期服务频道和半年频道两种版本都将由安全更新和非安全更新进行支持。 不同之处在于版本受支持的时间长度，如前文所述。

### <a name="servicing-tools"></a>维护工具

IT 专业人员可以使用多种工具维护 Windows Server。 每个选项均有其优点和缺点，范围从功能和控制到简洁性和管理要求低。 以下是可用于管理维护更新的维护工具示例：

- **Windows 更新（独立）** ：此选项仅适用于已连接到 Internet 并已启用 Windows 更新的服务器。
- **Windows Server Update Services (WSUS)** 可在大范围内控制 Windows 10 和 Windows Server 更新，并且内置在 Windows Server 操作系统中。 除了能够延迟更新，组织还可以添加用于更新的批准层，并在准备就绪后将它们部署到特定计算机或计算机组。
- **System Center Configuration Manager** 可最大程度地控制维护。 IT 专业人员可以延迟更新、批准更新，并且可以使用多种选项指向部署以及管理带宽使用情况和部署次数。

你可能已基于你的资源、员工和专业知识选择使用这些选项中的至少一项。 你可以继续将相同的流程用于半年频道版本：例如，如果你已使用 System Center Configuration Manager 管理更新，则可以继续使用。 同样，如果你正在使用 WSUS，也可以继续使用。

## <a name="where-to-obtain-semi-annual-channel-releases"></a>在何处获取半年频道版本

半年频道版本应作为干净安装产品进行安装。

- 批量许可服务中心 (VLSC)：享受[软件保障](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx)的批量许可客户可以转到[批量许可服务中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)并单击“登录”来获取此版本  。 然后，单击“下载和密钥”并搜索此版本  。 

- 半年频道版本也会在 [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview) 中提供。

- Visual Studio 订阅：Visual Studio 订阅者可以从 [Visual Studio 订阅者下载页](https://my.visualstudio.com/downloads?pid=2347)下载半年频道版本。 如果你还不是订阅者，请转到 [Visual Studio 订阅](https://www.visualstudio.com/subscriptions/)进行注册，然后访问上方所述的 [Visual Studio 订阅者下载页](https://my.visualstudio.com/downloads?pid=2347)。 通过 Visual Studio 订阅获得的版本仅用于开发和测试。

- 通过 Windows 预览体验计划获取预览版本：测试较早版本的 Windows Server 对 Microsoft 及其客户都有帮助，因为他们有机会在发布之前发现可能出现的问题。 此外，这还为客户提供了一个直接影响产品功能的独特机会。   
Microsoft 依赖于在开发过程中接收反馈，从而可以尽快做出调整。 提前测试和反馈对于快速发布模型至关重要。 若要参与 Windows 预览体验计划，请参阅 [Windows 预览体验计划服务器版文档](https://docs.microsoft.com/windows-insider/at-work/)。

## <a name="activating-semi-annual-channel-releases"></a>激活半年频道版本

- 如果你使用的是 Microsoft Azure，则此版本应该会自动激活。
- 如果你从批量许可服务中心或 Visual Studio 订阅获取了此版本，则可以使用你的 Windows Server 2016 CSVLK 通过你的密钥管理系统 (KMS) 环境来激活它。 有关详细信息，请参阅 [KMS 客户端安装密钥](../get-started/kmsclientkeys.md)。

在 Windows Server 2019 之前发布的半年频道版本使用 Windows Server 2016 CSVLK。

## <a name="why-do-semi-annual-channel-releases-offer-only-the-server-core-installation-option"></a>半年频道版本为何仅提供 Server Core 安装选项？

我们在规划 Windows Server 的每个版本时所采取的最重要步骤之一就是聆听客户的反馈意见 - 你如何使用 Windows Server？ 哪些新功能对你的 Windows Server 部署影响最大，扩展后，哪些对你的日常业务影响最大？ 你的反馈告诉我们，当务之急是尽快并且尽可能高效地提供创新。 同时，对于以最快速度进行创新的那些客户，你已经告诉我们，你主要将命令行脚本与 PowerShell 配合使用来管理数据中心，因此，对于桌面 GUI（在安装具有桌面体验的 Windows Server 时提供）的需求并不强烈，尤其是现已提供了 [Windows Admin Center](../manage/windows-admin-center/overview.md) 来远程管理服务器。

通过专注于 Server Core 安装选项，我们可以专门为这些创新投入更多资源，同时还保留传统的 Windows Server 平台功能和应用程序兼容性。 如果你有关于这方面的反馈或与 Windows Server 和我们的未来版本相关的其他问题，可以通过[反馈中心](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)提出建议和意见。

## <a name="what-about-nano-server"></a>Nano Server 的情况如何？

Nano Server 在半年频道中作为容器操作系统提供。 有关详细信息，请参阅[在 Windows Server 半年频道中对 Nano Server 所做的更改](../get-started/nano-in-semi-annual-channel.md)。

## <a name="how-to-tell-whether-a-server-is-running-an-ltsc-or-sac-release"></a>如何判断服务器运行的是 LTSC 还是 SAC 版本

一般而言，长期服务频道版本（如 Windows Server 2019）是在发布新版半年频道（如 Windows Server 版本 1809）的同时发布的。 这会略微增大确定服务器是否运行半年频道版本的难度。 不要查看内部版本号，而必须查看产品名称：半年频道版本使用“Windows Server Standard”或“Windows Server Datacenter”产品名称且不带版本号，而长期服务频道版本包括版本号，例如“Windows Server 2019 Datacenter”。

>[!Note]  
> 以下指南旨在帮助识别并区分 LTSC 和 SAC，且仅用于生命周期和常规清单目的，  而不用于应用程序兼容性或用于表示特定的 API 图面。  应用开发人员应使用其它指南，以在系统生命周期内添加组件、API 和功能之前或之时确保它们的兼容性。 [操作系统版本](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version)对于开发人员来说更好入手。

打开 Powershell，并使用 Get-ItemProperty Cmdlet 或 Get-ComputerInfo Cmdlet 检查注册表中的相应属性。  结合属性和内部版本号，即可确认品牌年度（如 2019 年）中是否存在 LTSC 或 SAC。  LTSC 拥有内部版本号，但 SAC 没有。  该命令还会返回版本发布时的 ReleaseId 或 WindowsVersion（如版本 1809），同时提示本次安装的是 Server Core 还是带桌面体验的服务器。 

**带桌面体验的 Windows Server 2019 Datacenter Edition (LTSC) 示例：**

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

**Windows Server 版本 1809 (SAC) Standard Edition Server Core 示例：**

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

**Windows Server 2019 Standard Edition (LTSC) Server Core 示例：**


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

若要查询新的 [Server Core 应用程序兼容性 FOD](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) 是否存在于服务器上，请使用 [Get-WindowsCapability](https://docs.microsoft.com/powershell/module/dism/get-windowscapability?view=win10-ps) Cmdlet 查找：
````
Name    :     ServerCore.AppCompatibility~~~~0.0.1.0
State   :     Installed
````

## <a name="see-also"></a>另请参阅

[Windows Server 半年频道中对 Nano Server 所做的更改](../get-started/nano-in-semi-annual-channel.md)

[Windows Server 支持生命周期](https://support.microsoft.com/lifecycle)

[确定 Server Core 是否正在运行](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[GetProductInfo 函数](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[软件清单日志记录 Cmdlet](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
