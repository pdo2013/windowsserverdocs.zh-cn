---
title: Windows Server 版本 1903 中的新增功能
description: 本主题介绍 Windows Server 版本 1903（半年频道发行版）中的一些新功能。
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: b8ef24aa62df0ea538a6ee34da17714ac9b4107f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391903"
---
# <a name="whats-new-in-windows-server-version-1903"></a>Windows Server 版本 1903 中的新增功能

>适用于：Windows Server（半年频道）

本主题介绍 Windows Server 版本 1903（半年频道发行版）中的一些新功能。 这些功能包括容器的运行和管理增强功能、在 Server Core 安装中使用的工具，以及从 Linux 设备迁移存储的功能。

若要了解 Windows Server 最新长期服务频道 (LTSC) 发行版中的新增功能，请参阅 [Windows Server 2019 中的新增功能](../get-started-19/whats-new-19.md)。 另请参阅 [Windows 10 版本 1903 IT 专业人员内容中的新增功能](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903)。

此发行版的系统要求与 Windows Server 2019 相同 — 请参阅[系统要求](../get-started-19/sys-reqs-19.md)了解详细信息。 若要查看最近删除的功能，请参阅[从 Windows Server 版本 1903 开始已删除或计划取代的功能](../get-started-19/removed-features-1903.md)

> [!NOTE]
> Windows 容器必须使用主机服务器所用的相同 Windows 版本或更低的版本。  例如，运行 Windows Server 版本 1903（内部版本 18342）发行版的主机服务器可以运行使用相同或更低版本和内部版本号的 Windows Server 容器（即使容器使用 Windows Insider 预览版）。 有关详细信息，请参阅 [Windows 容器版本兼容性](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility)。

## <a name="enhanced-support-for-non-microsoft-container-services"></a>增强了对非 Microsoft 容器服务的支持

我们增强了平台功能，以支持 Azure 容器服务和非 Microsoft 容器服务。

- 我们已将 CRI 容器化与主机计算服务 (HCS) 相集成，以支持 Azure 中的 Windows 容器和 Windows 上的 Linux 容器 (LCOW) 的 pod。
- 我们与 Kubernetes 社区合作，以实现 Windows 容器支持。 随着 Kubernetes v1.14 的发布，Windows Server 节点正式支持从 beta 版过渡到稳定版。 有关详细信息，请参阅 [Kubernetes 现在支持 Windows 容器](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/)。
- Tigera Calico for Windows 现已作为 Tigera Essentials 订阅的一部分推出正式版，它提供可以在 Linux/Windows 混合环境之间互操作的非叠加网络和网络策略。
- 我们提供为 Windows 容器提供可伸缩性改进增强叠加网络支持，包括通过最新版本的 Flannel 和 Kubernetes v1.14 来与 Kubernetes 集成。 有关详细信息，请参阅 [Kubernetes 中的 Windows 支持简介](https://kubernetes.io/docs/setup/windows/)。

## <a name="directx-hardware-acceleration-in-containers"></a>容器中的 DirectX 硬件加速

我们即将在 Windows 容器中实现 DirectX API 硬件加速支持，以支持使用本地图形处理单元 (GPU) 硬件进行机器学习 (ML) 推断等方案。 有关详细信息，请参阅[在 Windows 容器中整合 GPU 加速](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939)。

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>已更新容器标识和组托管服务帐户文档

我们在[组托管服务帐户](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts)文档中添加了更多示例和兼容性信息，并在 PowerShell 库中提供了[凭据规范 PowerShell 模块](https://www.powershellgallery.com/packages/CredentialSpec)。 有关详细信息，请参阅 [What's new for container identity](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151)（容器标识的新增功能）博客文章。

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>将任务计划程序和 Hyper-V 管理器添加到 Server Core 安装

在生产环境中使用 Windows Server 半年频道时，我们建议使用 Server Core 安装选项。 但是，Server Core 在默认情况下会忽略许多有用的管理工具。 可以通过安装应用兼容性功能来添加许多最常用的工具，但这仍会缺少一些工具。

因此，根据客户的反馈，我们在此版本中已将额外的两个工具添加到应用兼容性功能：任务计划程序 (taskschd.msc) 和 Hyper-V 管理器 (virtmgmt.msc)。

有关详细信息，请参阅 [Server Core 应用兼容性功能](../get-started-19/install-fod-19.md)。

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>存储迁移服务现在可迁移本地帐户、群集和 Linux 服务器

使用存储迁移服务可以更轻松地将服务器迁移到更高版本的 Windows Server。 它提供一个图形工具用于盘点服务器上的数据，然后将数据和配置传输到更新的服务器 — 在此过程中，应用或用户无需更改任何设置。

使用此 Windows Server 版本协调迁移时，我们已添加以下功能：

- 将本地用户和组迁移到新服务器
- 从故障转移群集迁移存储
- 从使用 Samba 的 Linux 服务器迁移存储
- 使用 Azure 文件同步更轻松地将已迁移的共享同步到 Azure 中
- 迁移到 Azure 等新网络

有关存储迁移服务的详细信息，请参阅[存储迁移服务概述](../storage/storage-migration-service/overview.md)。

## <a name="system-insights-disk-anomaly-detection"></a>System Insights 磁盘异常情况检测

[System Insights](../manage/system-insights/overview.md) 是一项预测分析功能，它可以在本地分析 Windows Server 系统数据，并提供服务器运行情况的见解。 它还内置了许多的功能，但我们已添加通过 Windows Admin Center 安装附加功能的功能，其中第一项功能就是磁盘异常情况检测。

磁盘异常情况检测新功能可以突出显示磁盘的行为何时与往常不同。  尽管不同的情况并不一定是坏事，但在排查系统上的问题时，查看这些异常瞬间可能会有所帮助。

此功能也适用于运行 Windows Server 2019 的服务器。

## <a name="windows-admin-center-enhancements"></a>Windows Admin Center 增强

新版 Windows Admin Center 现已推出，其中为 Windows Server 添加了新功能。 有关最新功能的信息，请参阅 [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md)。

## <a name="security-baseline-for-windows-10-and-windows-server"></a>Windows 10 和 Windows Server 的安全基线

现已推出 Windows 10 版本 1903 以及 Windows Server 版本 1903 的[安全配置基线设置](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/)草稿版。

## <a name="setupdiag"></a>SetupDiag
[SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) 版本 1.4.1 现已推出。

SetupDiag 是一个命令行工具，可以帮助诊断 Windows 更新失败的原因。 SetupDiag 通过搜索 Windows 安装程序日志文件工作。 搜索日志文件时，SetupDiag 使用一组规则来匹配已知问题。 在当前版本的 SetupDiag 中，rules.xml 文件中包含 53 条规则，运行 SetupDiag 时会解压缩此文件。 推出新版本的 SetupDiag 时，会更新 rules.xml 文件。

## <a name="update-rollback-improvements"></a>更新回滚改进

如果在安装最新驱动程序或质量更新后出现启动失败，服务器现在可以通过删除更新立即自动恢复。 如果在最近安装驱动程序质量更新后设备无法正常启动，Windows 现在会自动卸载更新，使设备能够恢复正常运行。

此功能要求服务器结合 [Windows 恢复环境](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)分区使用 Server Core 安装选项。

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Microsoft Defender 高级威胁防护 (ATP) 改进

Windows Server 包含 Microsoft Defender 高级威胁防护（有关详细信息，请参阅 [Windows Server 上的 Windows Defender 防病毒](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)）。 此发行版包括以下改进：

- [减小受攻击面](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction) – IT 管理员可以使用高级 Web 保护来配置设备，以便可以针对特定的 URL 和 IP 地址定义允许列表和拒绝列表。
- [下一代保护](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) – 控制措施经过扩展，可以防范勒索软件、凭据滥用以及通过可移动存储发起的攻击。
    - 完整性强制功能 – 启用远程运行时证明。
    - 防篡改功能 – 使用基于虚拟化的安全性将关键的 ATP 安全功能与 OS 和攻击者相隔离。
- Microsoft Defender ATP 下一代保护技术：
    - **高级机器学习**：已使用高级机器学习和 AI 模型进行改进，可以防范 apex 攻击者使用创新的漏洞利用技术、工具和恶意软件发起攻击。
    - **紧急爆发防护**：提供紧急爆发防护，在检测到新的病毒爆发时，可以使用新的智能功能自动更新设备。
    - **符合 ISO 27001 认证**：确保已分析云服务中存在的威胁、漏洞及其影响，并部署风险管理和安全控制机制。
    - **地理定位支持**：支持样本数据的地理定位和主权，以及可配置的保留策略。