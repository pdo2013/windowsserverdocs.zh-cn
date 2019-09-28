---
title: 管理 Windows Server
description: 了解有关管理 Windows Server 的工具、建议和指南
ms.prod: windows-server
ms.technology: manage
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 03/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: 880f8da5bfb872fba6fe4886198d932c91f4bf86
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370473"
---
# <a name="manage-windows-server"></a>管理 Windows Server

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

>[!TIP]
> 要查找有关较旧版 Windows Server 的信息？ 在 docs.microsoft.com 上查看我们的其他 [Windows Server 库](/previous-versions/windows/)。 也可以[搜索此站点](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)了解具体信息。

 <ul class="cardse panelContent cols cols3">
    <li>
        <a href="https://docs.microsoft.com/windows-insider/at-work-pro/wip-4-biz-feedback-hub">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h2>管理</h2>
                <p>在将 Windows Server 部署到你的环境（包括所需特性和功能的特定角色）后，下一步是管理这些服务器。 Windows Server 包含大量工具，可以帮助你了解 Windows Server 环境、管理特定服务器、微调性能，并最终自动执行许多管理任务。 </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li> 
</ul> 

## <a name="manage-windows-server-systems-and-environments"></a>管理 Windows Server 系统和环境
在很大程度上，用于管理 Windows Server 实例的工具，取决于已部署的系统类型（具有桌面体验还是服务器核心的 Windows Server)、物理计算机还是虚拟机，以及服务器所在的位置。 使用以下信息在 Windows Server 上执行基本管理任务。

使用下表确定何时要使用哪些工具。

| 我   | 安装和运行 Windows Admin Center | 在 Windows Server 上运行服务器管理器 | 在 Windows 10 上运行 RSAT 中的服务器管理器 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| 坐在 Windows 10 电脑前 | X  |                                      | X                                        |
| 坐在运行桌面体验的 Windows Server 系统前 | X | X | X |
| 坐在运行 Server Core 的 Windows Server 系统前 |X（在 Windows 10 上安装，用于管理 Server Core） | | X |
| 坐在远离 Windows Server 系统的位置 |X | | X |
| 坐在远离 Windows Server 系统的位置，但确实具有桌面体验 |X | 使用 RDS 远程连接到服务器，然后使用服务器管理器 | X |

除了下述工具，还可以使用[远程桌面服务](../remote/remote-desktop-services/welcome-to-rds.md)访问本地、远程和虚拟服务器。 然后，你可以使用服务器管理器执行管理任务。

### <a name="manage-on-premises-systems-remote-systems-and-systems-without-ui-with-windows-admin-center"></a>使用 Windows Admin Center 管理本地系统、远程系统和没有 UI 的系统
[Windows Admin Center](../manage/windows-admin-center/overview.md) 是基于浏览器的管理应用，可用于本地管理 Windows Server，而无需依赖 Azure 或云。 利用 Windows Admin Center，你可以完全控制服务器基础架构的各个方面，对于在未连接到 Internet 的专用网络上进行管理特别有用。 你可以在 Windows 10 或网关服务器上安装 Windows Admin Center，也可以在要管理的 Windows Server 系统上直接安装。

>[!NOTE]
>Windows Admin Center 是我们以前常说的“Project Honolulu”的正式名称。

### <a name="manage-on-premises-systems-with-server-manager"></a>使用服务器管理器管理本地系统
[服务器管理器](server-manager/server-manager.md)是包含在 Windows Server 的完全安装中的管理控制台。 （它不适用于没有 UI 的安装 - Server Core 不包括服务器管理器。）使用服务器管理器可以安装和删除服务器角色，添加和删除远程服务器，启动和停止服务，以及查看收集的环境相关数据。

### <a name="manage-remote-systems-and-systems-without-ui-with-remote-server-administration-tools-rsat"></a>使用远程服务器管理工具 (RSAT)，无需 UI 即可管理远程系统和系统
如果你的环境中包括服务器核心或远程服务器（本地或虚拟机）的安装，你可以使用[远程服务器管理工具 (RSAT)](../remote/remote-server-administration-tools.md)管理这些系统。 RSAT 包括服务器管理器，以便于你可以使用它来管理所有服务器。

> [!IMPORTANT]
> RSAT 可在 Windows 10 上运行。 你不能在 Windows Server 核心上安装 RSAT。

你还可以从命令行管理服务器核心安装。 请参阅[服务器核心中的基本管理任务](server-core/server-core-administer.md)。

### <a name="manage-updates-to-windows-server-systems"></a>管理对 Windows Server 系统的更新
你可以使用 [Windows Server Update Services (WSUS)](windows-server-update-services/get-started/windows-server-update-services-wsus.md) 管理和部署 Windows Server 环境中的系统更新。

## <a name="gather-information-about-your-environment"></a>收集有关环境的信息
你以管理员的身份做出的很多决定取决于与你的环境中的系统和用户有关的数据。 使用以下信息和工具收集该数据。

从 [在您的组织中配置 Windows 诊断数据](/windows/configuration/configure-windows-diagnostic-data-in-your-organization) 开始，获取有关可从 Windows 10 和 Windows Server 收集的诊断数据。

### <a name="setup-and-boot-event-collectionget-started-with-setup-and-boot-event-collectionmd"></a>[安装和启动事件收集](get-started-with-setup-and-boot-event-collection.md)
使用安装和启动事件收集，可以指定“收集器”计算机，该计算机可以收集在启动或执行安装过程时在其他计算机上发生的各种重要事件。 然后，可以使用事件查看器、消息分析器、Wevtutil 或 Windows PowerShell cmdlet 分析收集的事件。 

### <a name="software-inventory-logging-silsoftware-inventory-loggingget-started-with-software-inventory-loggingmd"></a>[软件清单日志记录 (SIL)](software-inventory-logging/get-started-with-software-inventory-logging.md)

Windows Server 中软件清单日志记录功能包含一组简单的 PowerShell cmdlet，可帮助服务器管理员检索其服务器上安装的 Microsoft 软件的列表。 它还提供了使用 HTTPS 协议通过网络定期收集数据并将此数据转发到目标 Web 服务器进行聚合的功能。 对该功能（主要是按小时进行收集和转发）的管理也可以通过 PowerShell 命令完成。

### <a name="user-access-logging-ualuser-access-loggingget-started-with-user-access-loggingmd"></a>[用户访问日志记录 (UAL)](user-access-logging/get-started-with-user-access-logging.md)

用户访问日志记录用于聚合在运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的计算机上记录到本地数据库的唯一客户端设备和用户请求事件。 然后，这些记录可用于（通过服务器管理员执行的查询）按服务器角色、用户、设备、本地服务器和日期检索数目和实例。 此外，UAL 还允许非 Microsoft 软件开发人员检测要聚合的 UAL 事件。 

## <a name="tune-your-windows-server-environment-for-performance"></a>为性能优化 Windows Server 环境
使用以下信息帮助你为性能优化环境。

### <a name="performance-tuning-guidelinesperformance-tuningindexmd"></a>[性能优化指南](performance-tuning/index.md)
查看一组可用于调整 Windows Server 2016 中的服务器设置和提高性能或能源效率的指南，尤其是工作负载的性质随时间的推移变化不大时。

### <a name="microsoft-server-performance-advisorserver-performance-advisormicrosoft-server-performance-advisormd"></a>[Microsoft Server Performance Advisor](server-performance-advisor/microsoft-server-performance-advisor.md)

使用 Microsoft Server Performance Advisor (SPA)，可以收集指标以悄悄诊断 Windows Server 上的性能问题，而不用添加软件代理或重新配置生产服务器。 SPA 可生成全面性能报告和历史记录图表以及建议。


## <a name="automate-windows-server-management"></a>自动管理 Windows Server

Windows Server 包含一组可用于自动执行管理任务的命令和 Windows PowerShell 模块。

### <a name="windows-powershellpowershellscriptingpowershell-scriptingviewpowershell-51"></a>[Windows PowerShell](/powershell/scripting/powershell-scripting?view=powershell-5.1)
Winows PowerShell 是命令行 shell 和脚本语言，旨在让你快速地自动执行管理任务。 

### <a name="windows-commandswindows-commandswindows-commandsmd"></a>[Windows 命令](windows-commands/windows-commands.md)

Windows 命令行工具用于在 Windows 中执行管理任务。 你可以使用命令参考熟悉命令行工具，以了解命令 shell，并通过使用批处理文件或脚本工具自动执行命令行任务。

## <a name="windows-server-insider-preview"></a>Windows Server Insider Preview
### <a name="system-insightsmanagesystem-insightsoverviewmd"></a>[系统见解](../manage/system-insights/overview.md)
系统见解是一项新功能，可在本机将预测分析引入到 Windows Server 中。 这些预测功能在本地分析 Windows Server 系统数据，例如性能计数器或 ETW 事件，并帮助 IT 管理员主动检测和解决其部署中的问题行为。 
