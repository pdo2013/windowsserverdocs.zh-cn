---
title: Management
description: 了解关于 Windows Server 管理的工具、建议和指南
ms.prod: windows-server-threshold
layout: LandingPage
ms.technology: manage
ms.topic: landing-page
author: lizap
ms.author: elizapo
ms.localizationpriority: high
ms.openlocfilehash: e6a5357e3e33b3d3318a3e281bbb5c80be842155
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890508"
---
# <a name="management"></a>Management


>[!TIP]
> 要查找有关较旧版 Windows Server 的信息？ 在 docs.microsoft.com 上查看我们的其他 [Windows Server 库](/previous-versions/windows/)。 也可以[搜索此站点](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)了解具体信息。

<hr />

在将 Windows Server 部署到你的环境（包括所需特性和功能的特定角色）后，下一步是管理这些服务器。 Windows Server 包含大量工具，可以帮助你了解 Windows Server 环境、管理特定服务器、微调性能，并最终自动执行许多管理任务。 

在很大程度上，用于管理 Windows Server 实例的工具，取决于已部署的系统类型（具有桌面体验还是服务器核心的 Windows Server)、物理计算机还是虚拟机，以及服务器所在的位置。 使用以下信息在 Windows Server 上执行基本管理任务。

使用下表确定何时要使用哪些工具。

| 我   | 安装和运行 Windows Admin Center | 在 Windows Server 上运行服务器管理器 | 在 Windows 10 上运行 RSAT 中的服务器管理器 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| 坐在 Windows 10 电脑前 | X  |                                      | X                                        |
| 坐在运行桌面体验的 Windows Server 系统前 | X | X | X |
| 坐在运行服务器核心的 Windows Server 系统前 |X（在 Windows 10 上安装，用于管理服务器核心） | | X |
| 坐在远离 Windows Server 系统的位置 |X | | X |
| 坐在远离 Windows Server 系统的位置，但确实具有桌面体验 |X | 使用 RDS 远程连接到服务器，然后使用服务器管理器 | X |

除了下述工具，你还可以使用[远程桌面服务](../remote/remote-desktop-services/welcome-to-rds.md)访问本地、远程和虚拟服务器。 然后，你可以使用服务器管理器执行管理任务。

<HR />

<ul class="cardsI panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>管理 Windows Server 系统和环境</h3>
<HR />
                        <p><h3><a href="../manage/windows-admin-center/overview.md">管理在本地系统、 远程系统和不含 UI 与 Windows Admin Center 的系统</a></h3>一款基于浏览器的管理应用，可用于在本地管理 Windows Server，而无需依赖 Azure 或云。 借助 Windows Admin Center（以前称为“Project Honolulu”），你可以完全控制服务器基础结构的各个方面，对于在未连接到 Internet 的专用网络上进行管理特别有用。 你可以在 Windows 10 或网关服务器上安装 Windows Admin Center，也可以在要管理的 Windows Server 系统上直接安装。</p>
<HR />
                        <p><h3><a href="server-manager/server-manager.md">管理在本地系统使用服务器管理器</a></h3>包含在 Windows Server 的完全安装中的管理控制台。 （不适用于不具有 UI 的安装-服务器核心不包括服务器管理器）。使用服务器管理器来安装和删除服务器角色添加和删除远程服务器、 开始和停止服务和查看数据收集的有关您的环境。</p>
<HR />
                        <p><h3><a href="../remote/remote-server-administration-tools.md">管理远程系统和不含 UI 与远程服务器管理工具 (RSAT) 的系统</a></h3>如果你的环境中包括安装服务器核心或远程服务器（本地或虚拟机），你可以使用 RSAT 管理这些系统。 RSAT 包括服务器管理器，以便于你可以使用它来管理所有服务器。 请注意，RSAT 可在 Windows 10 上运行。 你不能在 Windows Server 核心上安装 RSAT。 你还可以从命令行管理服务器核心安装。 请参阅<a href="server-core/server-core-administer.md">Server Core 中的基本管理任务</a>
<HR />
                        <p><h3><a href="windows-server-update-services/get-started/windows-server-update-services-wsus.md">管理更新部署到 Windows Server 系统</a></h3>使用 Windows Server Update Services (WSUS) 管理和部署对 Windows Server 环境中的系统的更新。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>收集有关环境的信息</h3>
<HR />
                        <p><h3><a href="get-started-with-setup-and-boot-event-collection.md">安装和启动事件收集</a></h3>使用安装和启动事件收集，你可以指定“收集器”计算机，该计算机可以收集在启动或执行安装过程时在其他计算机上发生的各种重要事件。 然后，你可以使用事件查看器、消息分析器、Wevtutil 或 Windows PowerShell cmdlet 分析收集的事件。 </p>
<HR />
                        <p><h3><a href="software-inventory-logging/get-started-with-software-inventory-logging.md">软件清单日志记录 (SIL)</a></h3>Windows Server 中软件清单日志记录功能包含一组简单的 PowerShell cmdlet，可帮助服务器管理员检索其服务器上安装的 Microsoft 软件的列表。 它还提供了使用 HTTPS 协议通过网络定期收集数据并将此数据转发到目标 Web 服务器进行聚合的功能。 对该功能（主要是按小时进行收集和转发）的管理也可以通过 PowerShell 命令完成。</p>
<HR />
                        <p><h3><a href="user-access-logging/get-started-with-user-access-logging.md">用户访问日志记录 (UAL)</a></h3>用户访问日志记录用于聚合在运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的计算机上记录到本地数据库的唯一客户端设备和用户请求事件。 然后，这些记录可用于（通过服务器管理员执行的查询）按服务器角色、用户、设备、本地服务器和日期检索数目和实例。 此外，UAL 还允许非 Microsoft 软件开发人员检测要聚合的 UAL 事件。 </a>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>为性能优化 Windows Server 环境</h3>
<HR />
                        <p><h3><a href="performance-tuning/index.md">性能优化指南</a></h3>查看一组可用于调整 Windows Server 中的服务器设置和提高性能或能源效率的指南，尤其是工作负载的性质随时间的推移变化不大时。</p>
<HR />
                        <p><h3><a href="server-performance-advisor/microsoft-server-performance-advisor.md">Microsoft Server Performance Advisor</a></h3>使用 Microsoft Server Performance Advisor (SPA)，你可以收集指标以悄悄诊断 Windows Server 上的性能问题，而不用添加软件代理或重新配置生产服务器。 SPA 可生成全面性能报告和历史记录图表以及建议。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>自动管理 Windows Server</h3>
<HR />
                        <p><h3><a href="https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-5.1">Windows PowerShell</a></h3>Windows PowerShell 是命令行 shell 和脚本语言，旨在让你快速地自动执行管理任务。 </p>
<HR />
                        <p><h3><a href="windows-commands/windows-commands.md">Windows 命令</a></h3>Windows 命令行工具用于在 Windows 中执行管理任务。 你可以使用命令参考熟悉命令行工具，以了解命令 shell，并通过使用批处理文件或脚本工具自动执行命令行任务。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>自动管理 Windows Server</h3>
<HR />
                        <p><h3><a href="..\manage\system-insights\overview.md">系统见解</h3></a>本机的预测分析本地分析 Windows Server 系统数据，例如性能计数器和 ETW 事件，主动帮助 IT 管理员检测和解决在部署中有问题的行为。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>