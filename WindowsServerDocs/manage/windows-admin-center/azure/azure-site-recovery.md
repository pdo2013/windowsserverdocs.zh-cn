---
title: 保护的 HYPER-V 虚拟机使用 Azure Site Recovery 和 Windows Admin Center
description: 通过 Azure Site Recovery 使用 Windows Admin Center (Project Honolulu) 保护 Hyper-V 虚拟机。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 66e9b2e23a60d1e4725321e88fc1ac262b9c31fa
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445921"
---
# <a name="protect-your-hyper-v-virtual-machines-with-azure-site-recovery-and-windows-admin-center"></a>保护的 HYPER-V 虚拟机使用 Azure Site Recovery 和 Windows Admin Center

>适用于：Windows Admin Center 预览版中，Windows Admin Center

[了解有关 Windows Admin Center 与 Azure 集成的详细信息。](../plan/azure-integration-options.md)

Windows Admin Center 简化了在 Hyper-V 服务器或群集上复制虚拟机的流程，让你能够更轻松地通过自己的数据中心使用 Azure 的强大功能。 若要自动设置，可以将 Windows Admin Center 网关连接到 Azure。

请使用以下信息来配置复制设置并从 Azure 门户内创建恢复计划，这样 Windows Admin Center 可以启动虚拟机复制并保护你的虚拟机。

## <a name="what-is-azure-site-recovery-and-how-does-it-work-with-windows-admin-center"></a>什么是 Azure Site Recovery？它如何与 Windows Admin Center 共同工作？ 

**Azure Site Recovery** 是一项 Azure 服务，它复制在虚拟机上运行的工作负载，以便在出现灾难时能够让你的业务关键基础结构受到保护。  [了解有关 Azure Site Recovery 的详细信息](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)。

Azure Site Recovery 由两个组件组成：**复制**和**故障转移**。 复制部分将目标虚拟机的 VHD 复制到 Azure 存储帐户，从而在灾难出现时保护虚拟机。 然后，你可以对这些虚拟机执行故障转移，在发生灾难时在 Azure 中运行它们。 你还可以在不影响主虚拟机的情况下执行测试故障转移来测试 Azure 中的恢复过程。

只是完成复制组件的设置就足以在灾难发生时保护你的虚拟机。 但是，在配置故障转移部分之前，你将无法在 Azure 中启动虚拟机。 当你希望故障转移到 Azure 虚拟机时可以设置故障转移部分 - 这不是初始设置中要求完成的部分。 如果主机服务器出现故障并且你尚未配置故障转移组件，那么你可以在此时进行设置，并访问受保护虚拟机的工作负载。 不过，在灾难发生前配置与故障转移相关的设置是一个好做法。
 

## <a name="prerequisites-and-planning"></a>先决条件和计划

- 托管你想保护的虚拟机的目标服务器必须能够访问 Internet 才可以复制到 Azure。
- [将你的 Windows Admin Center 网关连接到 Azure](azure-integration.md)。
- [查看容量计划工具以评估成功复制和故障转移的要求](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-capacity)。

## <a name="step-1-set-up-vm-protection-on-your-target-host"></a>第 1 步：设置目标主机上的虚拟机保护

> [!NOTE] 
> 在包含虚拟机的每个主机服务器或群集被确定为保护目标后，你需要执行此步骤。

1. 导航到托管你想要保护的虚拟机的服务器或群集（服务器管理器或超融合群集管理器）。
2. 转到**虚拟机** > **清单**。
3. 选择任意虚拟机（不需要是你想要保护的虚拟机）。
4. 选择**更多** > **设置虚拟机保护**。
5. 登录到你的 Azure 帐户。
6. 输入所需信息：

   - **订阅：** 想要使用此主机上的 Vm 复制的 Azure 订阅。
   - **位置：** 应在其中创建 ASR 资源的 Azure 区域。
   - **存储帐户：** 存储帐户保存复制的 VM 工作负荷在此主机上。
   - **保管库：** 为此主机上受保护的 Vm 选择 Azure Site Recovery 保管库的名称。

7. 选择**设置 ASR**。
8. 等待，直到您会看到通知：**站点恢复设置已完成**。
 
这最多可能需要 10 分钟。 你可以转到**通知**（右上方的钟铃图标）查看进度。

>[!NOTE]
> 此步骤会自动将 ASR 代理安装到目标服务器或节点上（如果在群集上配置），并使用指定的**存储帐户**和**保管库**在指定**位置**创建**资源组**。 另外还将使用 ASR 服务注册目标主机，并配置默认复制策略。

## <a name="step-2-select-virtual-machines-to-protect"></a>步骤 2：选择要保护的虚拟机

1. 导航回在上面的步骤 2 中配置的服务器或群集，并转到**虚拟机 > 清单**。
2. 选择你要保护的虚拟机。
3. 选择**更多** > **保护虚拟机**。
4. 查看[保护虚拟机的容量要求](https://docs.microsoft.com/azure/site-recovery/site-recovery-capacity-planner)。

    如果您想要使用高级存储帐户，[请在 Azure 门户中创建一个](https://docs.microsoft.com/azure/storage/common/storage-premium-storage)。 Windows Admin Center 窗格中提供的**新建**选项创建标准存储帐户。

5. 输入要用于此虚拟机复制的**存储帐户**的名称，然后选择**保护虚拟机**。 此步骤支持复制所选虚拟机。 

6. ASR 将开始复制。 当**虚拟机清单**网格的**受保护**列中的值更改为**是**时，复制完成、虚拟机受到保护。 这可能需要几分钟。  

## <a name="step-3-configure-and-run-a-test-failover-in-the-azure-portal"></a>步骤 3:配置和在 Azure 门户中运行测试故障转移

 虽然在开始虚拟机复制时无需完成此步骤（通过复制虚拟机已受到保护），但我们建议在设置 Azure Site Recovery 时配置故障转移设置。 如果你想要为 Azure 虚拟机的故障转移作准备，请完成以下步骤：

1. [设置 Azure 网络](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-prepare-azure)，故障转移的虚拟机会将其附加到此 VNET。 请注意，链接页面上列出的其他步骤会由 Windows Admin Center 自动完成；你只需设置 Azure 网络。

2. [运行测试故障转移](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-test-failover)。

## <a name="step-4-create-recovery-plans"></a>步骤 4：创建恢复计划

**恢复计划**是 Azure Site Recovery 中的一项功能，让你可以对包含一组虚拟机的整个应用程序执行故障转移和恢复。 虽然可以通过向恢复计划添加包含应用程序的虚拟机来单独恢复受保护的虚拟机，不过，你也可以通过恢复计划对整个应用程序执行故障转移。 你也可以使用恢复计划的测试故障转移功能来测试应用程序的恢复。 恢复计划让你可以对虚拟机分组，排列故障转移期间虚拟机的引发顺序，并恢复过程中自动执行附加步骤。 在虚拟机受到保护后，你可以转到 Azure 门户中的 Azure Site Recovery 保管库，并为这些虚拟机创建恢复计划。 [了解有关恢复计划的详细信息](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans)。

## <a name="monitoring-replicated-vms-in-azure"></a>监视 Azure 中复制的虚拟机 ##

若要验证服务器注册中没有故障，请转到 **Azure 门户** > **所有资源** > **恢复服务保管库**（你在步骤 2 中指定的保管库）> **作业** > **站点恢复作业**。

你可以转到**恢复服务保管库** > **复制的项目**来监视虚拟机复制。

若要查看注册到保管库的所有服务器，请转到**恢复服务保管库** > **站点恢复基础结构** > **Hyper-V 主机**（在 Hyper-V 站点部分下）。

## <a name="known-issue"></a>已知问题 ##

在向群集注册 ASR 时，如果节点未成功安装 ASR 或未注册到 ASR 服务，虚拟机可能不会受到保护。 验证在 Azure 门户中注册的群集的所有节点：转到**恢复服务保管库** > **作业** > **网站恢复作业**。