---
title: Windows Admin Center 常见问题解答
description: 获取有关 Windows Admin Center (Project Honolulu) 问题的解答
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.prod: windows-server
ms.openlocfilehash: 74d886246eb9d27264c0b8653f90f2eed86b891c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406820"
---
# <a name="windows-admin-center-frequently-asked-questions"></a>Windows Admin Center 常见问题解答

> 适用于：Windows Admin Center、Windows Admin Center 预览版

以下是有关 Windows Admin Center 最常见问题的解答。

## <a name="what-is-windows-admin-center"></a>什么是 Windows Admin Center？

Windows Admin Center 是一个轻型的基于浏览器的 GUI 平台及工具集，供 IT 管理员用来管理 Windows Server 和 Windows 10。 它从熟悉的内置管理工具（如服务器管理器和 Microsoft 管理控制台 (MMC)）演变而来，现在可提供现代化、简化、集成、安全的体验。

## <a name="can-i-use-windows-admin-center-in-production-environments"></a>我是否可以在生产环境中使用 Windows Admin Center？

是。 Windows Admin Center 通常可供广泛使用和进行生产部署。 当前的平台功能和核心工具达到了 Microsoft 的标准发布准则，以及我们的可用性、可靠性、性能、辅助功能、安全性和采用质量标准。

[!INCLUDE [support-policy](../includes/support-policy.md)]

## <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>使用 Windows Admin Center 需要多少费用？

Windows Admin Center 不会在 Windows 以外产生额外费用。 你可以通过有效的 Windows Server 或 Windows 10 许可证使用 Windows Admin Center（可单独下载），无额外费用 - 它已经过 Windows Supplemental EULA 许可。

## <a name="what-versions-of-windows-server-can-i-manage-with-windows-admin-center"></a>使用 Windows Admin Center 可以管理哪些版本的 Windows Server？

Windows Admin Center 将针对 Windows Server 2019 优化，可在 Windows Server 2019 版本中启用重要主题：特别是混合云方案和超融合基础设施管理。 虽然 Windows Admin Center 最适用于 Windows Server 2019，但它支持管理客户已使用的各个版本：完全支持 Windows Server 2012 和更高版本。 此外，受限的功能还可以管理 Windows Server 2008 R2。

## <a name="is-windows-admin-center-a-complete-replacement-for-all-traditional-in-box-and-rsat-tools"></a>Windows Admin Center 是否已完全取代了所有传统的内置和 RSAT 工具？

否。 尽管 Windows Admin Center 可以管理许多常见场景，但它没有完全取代所有传统的 Microsoft 管理控制台 (MMC) 工具。 若要详细了解 Windows Admin Center 附带了哪些工具，请详细阅读我们文档中的[管理服务器](../use/manage-servers.md)内容。 Windows Admin Center 在其服务器管理器解决方案中提供以下主要功能：

* 显示资源和资源使用率
* 证书管理
* 管理设备
* 事件查看器
* 文件资源管理器
* 防火墙管理
* 管理已安装的应用
* 配置本地用户和组
* 网络设置
* 查看/结束进程和创建进程转储
* 编辑注册表
* 管理计划的任务
* 管理 Windows 服务
* 启用/禁用角色和功能
* 管理 Hyper-V VM 和虚拟交换机
* 管理存储
* 管理存储副本
* 管理 Windows 更新
* PowerShell 控制台
* 远程桌面连接

Windows Admin Center 还提供了以下解决方案：

* 计算机管理 - 提供了一部分用于管理 Windows 10 客户端电脑的服务器管理器功能
* 故障转移群集管理器 - 提供对持续管理故障转移群集和群集资源的支持
* 超融合群集管理器 - 提供专为存储空间直通和 Hyper-V 定制的全新体验。 它采用仪表板并重点使用图表和警报进行监视。

Windows Admin Center 是对 RSAT（远程服务器管理工具）的补充，不会取代它，因为角色（如 Active Directory、DHCP、DNS、IIS）在 Windows Admin Center 中还没有等效的管理功能。

## <a name="can-windows-admin-center-be-used-to-manage-the-free-microsoft-hyper-v-server"></a>Windows Admin Center 能否用于管理免费的 Microsoft Hyper-V Server？

是。 Windows Admin Center 可用于管理 Microsoft Hyper-V Server 2016 和 Microsoft Hyper-V Server 2012 R2。

## <a name="can-i-deploy-windows-admin-center-on-a-windows-10-computer"></a>我是否可以在 Windows 10 计算机上部署 Windows Admin Center？

可以，Windows Admin Center 可以安装在 Windows 10（版本 1709 或更高版本）上，在桌面模式下运行。  而且，Windows Admin Center 还可以在网关模式下安装在包含 Windows Server 2016 或更高版本的服务器上，然后通过 Windows 10 计算机的 Web 浏览器访问。 [了解有关安装选项的详细信息](../plan/installation-options.md)。

## <a name="ive-heard-that-windows-admin-center-uses-powershell-under-the-hood-can-i-see-the-actual-scripts-that-it-uses"></a>我听说 Windows Admin Center 实质上使用的是 PowerShell，是否可以看到它使用的实际脚本？

可以！ [Showscript 功能](../use/get-started.md#view-powershell-scripts-used-in-windows-admin-center)已添加到 Windows Admin Center 预览版 1806 中，现已包含在 GA 频道中。

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-windows-server-2008-r2-or-earlier"></a>是否有 Windows Admin Center 管理 Windows Server 2008 R2 或更早版本的计划？

Windows Admin Center 现在支持**有限的**功能来管理 Windows Server 2008 R2。 Windows Admin Center 依赖于 Windows Server 2008 R2 及更早版本中没有的 PowerShell 功能和平台技术，这让完全支持无法实现。 Windows Server 2008/2008 R2 将于 2020 年 1 月终止支持，因此 Microsoft 建议客户[过渡到 Azure 或升级到最新版本的 Windows Server](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008)。

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-linux-connections"></a>是否有 Windows Admin Center 管理 Linux 连接的计划？

应客户要求，我们正在调查，但目前没有要提供的锁定计划，支持可能仅包含通过 SSH 的控制台连接。

## <a name="which-web-browsers-are-supported-by-windows-admin-center"></a>Windows Admin Center 支持哪些 Web 浏览器？

Microsoft Edge 的最新版本（Windows 10 版本 1709 或更高版本）和 Google Chrome 浏览器已通过测试，在 Windows 10 上受支持。 [查看浏览器特定的已知问题](../support/known-issues.md#browser-specific-issues)。 其他现代的 Web 浏览器或其他平台当前不在我们的测试矩阵中，因此不受*正式*支持。

## <a name="how-does-windows-admin-center-handle-security"></a>Windows Admin Center 如何处理安全问题？

浏览器到 Windows Admin Center 网关的通信使用 HTTPS。 网关到托管服务器的通信是标准 PowerShell 并通过 WinRM 管理 WMI。 我们支持使用 LAPS（本地管理员密码解决方案）、基于资源的约束委派、使用 AD 或 Azure AD 的网关访问控制和基于角色的访问控制来管理目标服务器。

## <a name="does-windows-admin-center-use-credssp"></a>Windows Admin Center 是否使用 CredSSP？

是的，在少数情况下，Windows Admin Center 需要 CredSSP。 需要使用 CredSSP 才能将用于身份验证的凭据传递给除用作管理目标的特定服务器以外的计算机。 例如，如果你在**服务器 B** 上管理虚拟机，但想要将这些虚拟机的 vhdx 文件存储在**服务器 C** 托管的文件共享上，则 Windows Admin Center 必须使用 CredSSP 在**服务器 C** 上进行身份验证才能访问该文件共享。

在提示你许可后，Windows Admin Center 会自动处理 CredSSP 的配置。 在配置 CredSSP 之前，Windows Admin Center 将检查以确保系统包含最新的 CredSSP [更新](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。 启用 CredSSP 时，“服务器概述”中会显示一条锁屏提醒，以及一个用于禁用 CredSSP 的选项 -

![服务器概述中的 CredSSP](../media/CredSSP-overview.png)

CredSSP 当前用于以下场合：

- 在虚拟机工具中使用非聚合 SMB 存储（以上示例）。
- 在执行[群集感知更新](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating)的故障转移或超融合群集管理解决方案中使用“更新”工具 

## <a name="are-there-any-cloud-dependencies"></a>是否有云依赖关系？

Windows Admin Center 不需要 Internet 访问，而且不需要 Microsoft Azure。 Windows Admin Center 在任意位置管理 Windows Server 和 Windows 实例：物理系统上，或任何虚拟机监控程序上的虚拟机中，或在任何云中运行。 随着时间的推移，将增加与各种 Azure 服务的集成，这些是可选的增值功能，不是使用 Windows Admin Center 的必要要求。

## <a name="are-there-any-other-dependencies-or-prerequisites"></a>是否有其他依赖关系或先决条件？

Windows Admin Center 可以安装在 Windows 10 Fall Anniversary Update (1709) 或更高版本中，或 Windows Server 2016 或更高版本中。 若要管理 Windows Server 2008 R2、2012 或 2012 R2，需要在这些服务器上安装 Windows Management Framework 5.1。 没有任何其他依赖关系。 不需要 IIS，不需要代理，也不需要 SQL Server。

## <a name="what-about-extensibility-and-3rd-party-support"></a>可扩展性和第三方支持方面怎么样？

Windows Admin Center 提供一个 SDK，可让任何人编写自己的扩展。 作为平台，发展我们的生态系统、支持合作伙伴可扩展性在最初就是重中之重。 [了解关于 Windows Admin Center SDK 的详细信息](../extend/extensibility-overview.md)。

## <a name="can-i-manage-hyper-converged-infrastructure-with-windows-admin-center"></a>是否可以使用 Windows Admin Center 管理超融合基础设施？

是。 Windows Admin Center 支持管理运行 Windows Server 2016 或 Windows Server 2019 的超融合群集。 Windows Admin Center 中的超融合群集管理器解决方案以前以预览版提供，但现在已推出**正式版**，其中保留了预览版中的一些新功能。 有关详细信息，[请阅读有关超融合基础设施的详细信息](../use/manage-hyper-converged.md)。

## <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center 是否需要 System Center？

否。 Windows Admin Center 是对 System Center 的补充，但不需要 System Center。 [阅读有关 Windows Admin Center 和 System Center 的详细信息](related-management.md#system-center)。

## <a name="can-windows-admin-center-replace-system-center-virtual-machine-manager-scvmm"></a>Windows Admin Center 能否取代 Center Virtual Machine Manager (SCVMM)？

Windows Admin Center 和 SCVMM 是补充功能；Windows Admin Center 的目标是取代传统的 Microsoft 管理控制台 (MMC) 管理单元和服务器管理体验。  Windows Admin Center 不会取代对 SCVMM 各方面的监视。 [阅读有关 Windows Admin Center 和 System Center 的详细信息](related-management.md#system-center)。

## <a name="what-is-windows-admin-center-preview-which-version-is-right-for-me"></a>什么是 Windows Admin Center 预览版？哪个版本适合我？

有两个版本的 Windows Admin Center 可供下载：

### <a name="windows-admin-center"></a>Windows Admin Center

* 对于不能经常更新或需要更多时间来验证在生产中使用的版本的 IT 管理员，应该选择这个版本。 当前的正式版 (GA) 为 Windows Admin Center 1904。
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* 若要获取最新版本，[请在此处下载](https://aka.ms/WACDownload)。

### <a name="windows-admin-center-preview"></a>Windows Admin Center 预览版

* 对于希望定期体验最新和最强大功能的 IT 管理员，应该选择这个版本。 我们打算每隔大约一个月提供后续更新版本。 核心平台仍然是生产就绪状态，许可证提供生产使用权限。 但是，请注意，后续将会引入被明确标注为“预览”的新工具和功能，它们适合用于评估和测试。
* 要获取最新的 Insider 预览版，注册的预览体验成员可以直接从 [Windows Server Insider Preview 下载页面](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)下载 Windows Admin Center 预览版，位于“其他下载”下拉列表下。 如果你尚未注册成为预览体验成员，请参阅适用于企业的 Windows 预览体验成员门户上的 [Windows Server 入门](https://insider.windows.com/en-us/for-business-getting-started-server/)。

## <a name="why-was-windows-admin-center-chosen-as-the-final-name-for-project-honolulu"></a>为何选择“Windows Admin Center”作为“Project Honolulu”的最终名称？

Windows Admin Center 是“Project Honolulu”的官方产品名称，强调了我们对一系列核心管理场景下 IT 管理员的集成体验的愿景。 它还突出了我们对于 IT 管理员用户需求的客户导向，将其作为我们投资和交付的中心。

## <a name="where-can-i-learn-more-about-windows-admin-center-or-get-more-details-on-the-topics-above"></a>在哪里可以了解有关 Windows Admin Center 的更多信息或获取有关上述主题的更多详细信息？

我们的[启动页面](https://aka.ms/WindowsAdminCenter)是最好的出发点，上面提供了访问我们新分类的文档内容、下载位置、如何提供反馈、参考信息和其他资源的链接。

## <a name="what-is-the-version-history-of-windows-admin-center"></a>什么是 Windows Admin Center 版本历史记录？

[查看此处的版本历史记录。](../overview.md#release-history)

## <a name="im-having-an-issue-with-windows-admin-center-where-can-i-get-help"></a>我有有关 Windows Admin Center 的问题，在哪里可以获得帮助？

请参阅我们的[故障排除指南](../use/troubleshooting.md)和[已知问题](../use/known-issues.md)列表。
