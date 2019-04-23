---
title: 受防护的 VM - 主机托管服务提供商设置 Windows Azure Pack
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9ca9f41770c977b6e7c4900b090471dbfe11a450
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855718"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>受防护的 VM - 主机托管服务提供商设置 Windows Azure Pack

本主题介绍如何托管服务提供商可以配置 Windows Azure 包，以便租户可以使用它来部署受防护的 Vm。 Windows Azure Pack 是扩展功能的 System Center Virtual Machine Manager 以允许租户部署和管理其自身的 Vm 通过简单的 web 界面的 web 门户。 Windows Azure Pack 完全支持受防护的 Vm，并使其更容易为租户创建和管理其防护数据文件。

若要了解本主题如何适应总体部署受防护的 Vm 的过程，请参阅[托管服务提供程序的配置步骤受保护的主机和受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="setting-up-windows-azure-pack"></a>设置 Windows Azure Pack

您将完成下列任务以在你的环境中设置 Windows Azure 包：

1. 完成配置 System Center 2016-Virtual Machine Manager (VMM) 托管构造的。 这包括设置虚拟机模板和 VM 云，将通过 Windows Azure Pack 公开：

    [方案-部署受保护的主机和受防护的虚拟机在 VMM 中](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. 安装和配置 System Center 2016-Service Provider Foundation (SPF)。 此软件将启用 Windows Azure 包与 VMM 服务器进行通信：

    [部署 Service Provider Foundation-SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. 安装 Windows Azure 包，并将其配置为与 SPF 通信：

    - [安装 Windows Azure Pack](#install-windows-azure-pack) （在本主题中）
    - [配置 Windows Azure Pack](#configure-windows-azure-pack) （在本主题中）

4. 在 Windows Azure Pack 来允许访问 VM 云的租户中创建一个或多个托管计划：

    [在 Windows Azure 包中创建一个计划](#create-a-plan-in-windows-azure-pack)（在本主题中）

## <a name="install-windows-azure-pack"></a>安装 Windows Azure 包

安装并想要为你的租户托管在 web 门户的计算机上配置 Windows Azure Pack (WAP)。 此计算机需要能够访问 SPF 服务器并让你的租户访问。

1.  审阅[WAP 系统要求](https://technet.microsoft.com/library/dn296442.aspx)并安装[必备软件](https://technet.microsoft.com/library/dn469335.aspx)。

2.  下载并安装[Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)。 如果计算机未连接到 Internet，请按照[脱机安装说明](http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release)。

3.  打开 Web 平台安装程序并找到**Windows Azure 包：门户和 API Express**下**产品**选项卡。单击**外**，然后**安装**在窗口的底部。

4.  继续执行安装。 安装完成后，配置网站 (*https://&lt;wapserver&gt;: 30101 /*) 将在 web 浏览器中打开。 此网站上提供有关您的 SQL server 的信息并完成配置 WAP。

安装 Windows Azure Pack 的帮助设置，请参阅[安装 Windows Azure Pack 的快速部署](https://technet.microsoft.com/dn296439.aspx)。

> [!NOTE]
> 如果你已在环境中运行 Windows Azure 包，可以使用你的现有安装。 若要使用最新受防护的 VM 功能，但是，需要至少升级到安装更新汇总 10。

### <a name="configure-windows-azure-pack"></a>配置 Windows Azure 包

使用 Windows Azure Pack 之前，您应该已经安装，并为您的基础结构配置。

1.  导航到 Windows Azure 包管理员门户，网址*https://&lt;wapserver&gt;: 30091*，然后使用你的管理员凭据登录。

2.  在左窗格中，单击**VM 云**。

3.  通过单击连接到 Service Provider Foundation 实例的 Windows Azure Pack**注册 System Center Service Provider Foundation**。 您需要指定 Service Provider Foundation 的 URL，以及用户名和密码。

    ![注册 System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  完成后，您应能够看到在您的 VMM 环境中进行设置的 VM 云。 确保具有至少一个 VM 云的支持受防护的 Vm 供 WAP 然后再继续。

### <a name="create-a-plan-in-windows-azure-pack"></a>在 Windows Azure Pack 中创建计划

若要允许租户在 WAP 中创建 Vm，必须先创建托管计划，租户可以订阅。 计划为你的租户定义允许的 VM 云、 模板、 网络和计费实体。

1.  在门户的下部窗格中，单击 **+ 新建** &gt; **计划** &gt; **创建计划**。

2.  在向导的第一个步骤中，选择你的计划的名称。 这是在订阅时，将看到你的租户的名称。

3.  在第二个步骤中，选择**虚拟机云**作为要在计划中提供的服务之一。

4.  跳过有关选择任何加载项计划的步骤。

5.  单击**确定**（复选标记） 以创建计划。 尽管这会创建该计划，但不是尚未处于配置状态。

    ![在 Windows Azure 中的计划包](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6.  若要开始配置该计划，请单击其名称。

7.  在下一页上，在**规划服务**，单击**虚拟机云**。 这会打开可在其中配置此计划的配额页。

8.  下**基本**，选择 VMM 管理服务器和你想要提供给租户的虚拟机云。 可以提供受防护的 Vm 的云将显示与 **（支持屏蔽）** 其名称旁边。

9.  选择想要在此计划中应用的配额。 （例如，在上个 CPU 核心和 RAM 使用情况限制）。 请务必选中**允许虚拟机到防护**选中复选框。

    ![在 Windows Azure Pack 中的虚拟机云的设置](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10.  向下的滚动至一节**模板**，然后选择一个或多个模板，以提供给租户。 您可以提供受防护的同时，必须提供给租户，未受防护的模板，但受防护的模板来为租户提供有关虚拟机和自己的机密的完整性的端到端保证。

11.  在中**网络**部分中，为你的租户中添加一个或多个网络。

12.  设置后的任何其他设置或配额的计划，单击**保存**底部。

13.  在屏幕的右下方，单击箭头以将您带回**计划**页。

14.  在屏幕的底部，从要更改计划**私有**到**公共**，以便租户可以订阅该计划。

    ![更改 Windows Azure Pack 中的计划的访问权限](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    在此情况下，Windows Azure 包配置和租户能够订阅到刚创建的计划和部署受防护的 Vm。 租户需要完成的其他步骤，请参阅[租户-使用 Windows Azure Pack 部署受防护的 VM 的受防护的 Vm](guarded-fabric-shielded-vm-windows-azure-pack.md)。

## <a name="see-also"></a>请参阅

- [托管服务提供程序的配置步骤受保护的主机和受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的构造和受防护的 Vm](guarded-fabric-and-shielded-vms-top-node.md)
