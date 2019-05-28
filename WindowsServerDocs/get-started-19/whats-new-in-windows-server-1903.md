---
title: What's New in Windows Server 版本 1903年
description: 本主题介绍了一些 Windows Server 版本 1903，它是半年频道发布的新功能。
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: 2aa84df1ca162cb6e2dfa580b4b0744ff08cc95a
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983437"
---
# <a name="whats-new-in-windows-server-version-1903"></a>什么是 Windows Server 版本 1903年中的新增功能

>适用于：Windows Server（半年频道）

本主题介绍了一些 Windows Server 版本 1903，它是半年频道发布的新功能。 这些功能包括用于运行和管理容器、 服务器核心安装中工作的工具和从 Linux 设备将迁移存储的功能的增强功能。

若要改为找出哪些是最新的长期服务频道 (LTSC) 版本的 Windows Server 中的新增功能，请参阅请参阅[What's New in Windows Server 2019](../get-started-19/whats-new-19.md)。 另请参阅[什么是 Windows 10，版本 1903 IT 专业人员内容中的新增功能](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903)。

对于此版本的系统要求是与 Windows Server 2019 相同 — 请参阅[系统要求](../get-started-19/sys-reqs-19.md)的详细信息。 若要了解什么最近已删除，请参阅[删除功能或从 Windows Server，版本 1903年开始替换的已计划](../get-started-19/removed-features-1903.md)

> [!NOTE]
> Windows 容器必须使用相同版本的 Windows 主机服务器或*早期*版本。 例如，运行发行的版本的 Windows Server 的主机服务器，版本 1903年 （内部 18342） 可以使用相同或早期版本运行 Windows Server 容器和内部版本号 （即使容器使用的 Windows Insider Preview 版本）。 有关详细信息，请参阅[Windows 容器版本兼容性](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility)。

## <a name="enhanced-support-for-non-microsoft-container-services"></a>增强了对非 Microsoft 容器服务支持

我们增强了平台功能来支持 Azure 容器服务和非 Microsoft 容器服务。

- 我们集成 CRI containerd 使用主机计算服务 (HCS) 以支持在 Windows (LCOW) 在 Azure 上的 Windows 容器和 Linux 容器的 pod。
- 我们合作 Kubernetes 社区，以启用 Windows 容器支持。 Kubernetes v1.14 的版本中，Windows Server 节点支持正式毕业于 beta 稳定的。 有关详细信息，请参阅[现在支持在 Kubernetes 中的 Windows 容器](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/)。
- Windows 的 Tigera Calico 现已公开发布为 Tigera Essentials 订阅的一部分和产品/服务都可跨互操作的非覆盖网络和网络策略混合 Linux/Windows 环境。
- 我们提供了增强覆盖网络用于 Windows 容器，包括与 kubernetes 配合使用通过最新版本的 Flannel 和 Kubernetes v1.14 的集成，以及支持的可伸缩性改进。 有关详细信息，请参阅[简介在 Kubernetes 中的 Windows 支持](https://kubernetes.io/docs/setup/windows/)。

## <a name="directx-hardware-acceleration-in-containers"></a>在容器中的 DirectX 硬件加速

我们启用硬件加速的 DirectX Api 在 Windows 容器 tp 支持方案，例如机器学习 (ML) 推断使用本地图形处理单元 (GPU) 硬件的支持。 有关详细信息，请参阅[到 Windows 容器将 GPU 加速](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939)。

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>更新的容器身份和组托管服务帐户文档

我们添加了更多示例和兼容性信息[组托管服务帐户](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts)文档，并进行[凭据规范 PowerShell 模块](https://www.powershellgallery.com/packages/CredentialSpec)PowerShell 库中提供。 有关详细信息，请参阅[What's new for 容器标识](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151)博客文章。

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>将任务计划程序和 Hyper-v 管理器添加到服务器核心安装

您可能知道，我们建议使用 Windows Server 中，在生产环境中的半年频道时使用的服务器核心安装选项。 但是，默认情况下的服务器核心中省略了许多有用的管理工具。 您可以添加许多最常用工具的安装应用程序兼容性功能，但仍已一些缺少的工具。

因此，根据客户反馈，我们添加两个更多工具到此版本中的应用程序兼容性功能：任务计划程序 (taskschd.msc) 和 HYPER-V 管理器 (virtmgmt.msc)。

有关详细信息，请参阅[服务器核心应用程序兼容性功能](../get-started-19/install-fod-19.md)。

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>本地帐户、 群集和 Linux 服务器，现在将迁移存储迁移服务

存储迁移服务，可以更轻松地迁移到较新版本的 Windows Server 的服务器。 它提供了一个图形工具，列出服务器上的数据的清单，然后将数据和配置传输至较新的服务器，同时应用程序或用户无需更改任何内容。

当使用此版本的 Windows Server 协调迁移，我们添加了以下功能：

- 将本地用户和组迁移到新服务器
- 从故障转移群集迁移存储
- 使用 Samba 的 Linux 服务器从迁移存储
- 更轻松地使用 Azure 文件同步到 Azure 中同步已迁移的共享
- 将迁移到 Azure 等的新网络

有关存储迁移服务的详细信息，请参阅[存储迁移服务概述](../storage/storage-migration-service/overview.md)。

## <a name="system-insights-disk-anomaly-detection"></a>系统 Insights 磁盘异常情况检测

[系统 Insights](../manage/system-insights/overview.md)是预测分析功能，本地 Windows Server 系统数据进行分析，并提供深入的服务器的运行状况。 它还提供了许多内置功能，但我们已添加可以安装其他功能通过 Windows Admin Center，以从磁盘异常情况检测。

磁盘异常情况检测是当磁盘的运行情况符合突出显示的新功能*以不同方式*比平常。 虽然不同并不一定是您的系统上进行问题排查时可以提供帮助是坏事，查看这些异常的情形。

此功能也是可用于运行 Windows Server 2019 的服务器。

## <a name="windows-admin-center-enhancements"></a>Windows Admin Center 增强功能

Windows Admin Center 的新版本，是添加到 Windows Server 的新功能。 有关最新功能的信息，请参见[Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md)。

## <a name="security-baseline-for-windows-10-and-windows-server"></a>适用于 Windows 10 和 Windows Server 的安全基准

草稿版本[安全配置基线设置](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/)适用于 Windows 10 版本 1903，及适用于 Windows Server 版本 1903年才可用。

## <a name="setupdiag"></a>SetupDiag
[SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag)版本 1.4.1 已推出。

SetupDiag 是一个命令行工具，可帮助诊断 Windows 更新失败的原因。 SetupDiag 通过搜索 Windows 安装程序日志文件工作。 搜索日志文件时，SetupDiag 使用一组规则来匹配已知问题。 在 SetupDiag 有 53 rules.xml 文件中包含的规则的当前版本中，从中提取 SetupDiag 运行时。 当推出新版本的 SetupDiag 时，将更新 rules.xml 文件。

## <a name="update-rollback-improvements"></a>更新回滚改进

服务器可以立即从故障自动恢复启动通过删除更新，如果在安装新的驱动程序或质量更新后引入了启动失败。 当设备无法正常启动的驱动程序更新质量最新安装完成后，Windows 将立即自动卸载要让备份设备的更新和正常运行。

此功能要求服务器使用服务器核心安装选项与[Windows 恢复环境](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)分区。

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Microsoft Defender 高级威胁防护 (ATP) 的改进

Windows Server 包含 Microsoft Defender 高级线程保护 (有关详细信息，请参阅[Windows Server 上的 Windows Defender 防病毒](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016))。 此版本包括以下改进：

- [攻击外围应用缩减](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction)– IT 管理员可以配置具有高级的 web 保护，使他们能够定义的设备允许和拒绝列表，为特定的 URL 和 IP 地址。
- [下一代保护](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)– 控件具有扩展到保护免受勒索软件、 凭据被误用和通过可移动存储传输的攻击。
    - 完整性强制功能 – 启用远程运行时证明。
    - 防篡改攻击的功能-使用基于虚拟化的安全隔离使 OS 和攻击者的关键 ATP 安全功能。
- Microsoft Defender ATP 下一代保护技术：
    - **高级机器学习**:通过高级的机器学习和 AI 模型，使其可以防止顶点攻击者使用创新的漏洞攻击技术、 工具和恶意软件得到改进。
    - **紧急爆发保护**:提供将自动更新的设备与新的信息时检测到新的病毒发作紧急爆发保护。
    - **ISO 27001 认证符合性**:可确保云服务已分析的威胁、 漏洞和影响，且风险管理和安全控件已准备就绪。
    - **地理位置支持**:支持地理位置和所有权的示例数据，以及可配置的保留策略。