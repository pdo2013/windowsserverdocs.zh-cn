---
title: 受防护的 VM - 主机托管服务提供商设置 Windows Azure Pack
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d388da2b7416543c307bd931636902b4a7543e1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403659"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>受防护的 VM - 主机托管服务提供商设置 Windows Azure Pack

本主题介绍托管服务提供商如何配置 Windows Azure Pack，以便租户可以使用它来部署受防护的 Vm。 Windows Azure Pack 是一种 web 门户，它扩展了 System Center Virtual Machine Manager 的功能，以允许租户通过简单的 web 界面来部署和管理自己的 Vm。 Windows Azure Pack 完全支持受防护的 Vm，并使租户更方便地创建和管理其屏蔽数据文件。

若要了解本主题如何适应部署受防护的 Vm 的整个过程，请参阅[为受保护的主机和受防护的 Vm 托管服务提供商配置步骤](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="setting-up-windows-azure-pack"></a>设置 Windows Azure Pack

你将完成下列任务以在你的环境中设置 Windows Azure Pack：

1. 完成托管构造的 System Center 2016-Virtual Machine Manager （VMM）的配置。 这包括设置 VM 模板和 VM 云，该云将通过 Windows Azure Pack 公开：

    [方案-在 VMM 中部署受保护的主机和受防护的虚拟机](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. 安装和配置 System Center 2016-Service Provider Foundation （SPF）。 此软件允许 Windows Azure Pack 与 VMM 服务器进行通信：

    [部署 Service Provider Foundation-SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. 安装 Windows Azure Pack 并将其配置为与 SPF 通信：

    - [安装 Windows Azure Pack](#install-windows-azure-pack) （在本主题中）
    - [配置 Windows Azure Pack](#configure-windows-azure-pack) （在本主题中）

4. 在 Windows Azure Pack 中创建一个或多个托管计划，以允许租户访问 VM 云：

    在[Windows Azure Pack 中创建计划](#create-a-plan-in-windows-azure-pack)（在本主题中）

## <a name="install-windows-azure-pack"></a>安装 Windows Azure Pack

在要为租户托管 web 门户的计算机上安装和配置 Windows Azure Pack （WAP）。 此计算机将需要能够访问 SPF 服务器，并可由租户访问。

1.  查看[WAP 系统要求](https://technet.microsoft.com/library/dn296442.aspx)并安装[必备软件](https://technet.microsoft.com/library/dn469335.aspx)。

2.  下载并安装[Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)。 如果计算机未连接到 Internet，请按照[脱机安装说明](http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release)进行操作。

3.  打开 Web 平台安装程序并查找 @no__t 0Windows Azure 包："**产品**" 选项卡下的门户和 API Express @ no__t。单击 "**添加**"，然后在窗口底部**安装**。

4.  继续执行安装。 安装完成后，会在 web 浏览器中打开配置网站（*https://&lt;wapserver @ no__t-2： 30101/* ）。 在此网站上，提供有关 SQL server 的信息并完成 WAP 的配置。

有关设置 Windows Azure Pack 的帮助，请参阅[安装 Windows Azure Pack 的快速部署](https://technet.microsoft.com/dn296439.aspx)。

> [!NOTE]
> 如果已在环境中运行 Windows Azure Pack，则可以使用现有安装。 但是，若要使用最新的受防护的 VM 功能，你需要将安装升级到至少更新汇总10。

### <a name="configure-windows-azure-pack"></a>配置 Windows Azure Pack

在使用 Windows Azure Pack 之前，你应该已为基础结构安装并配置了它。

1.  导航到 https://上的 Windows Azure Pack 管理门户 *&lt;wapserver @ no__t-2： 30091*，然后使用管理员凭据登录。

2.  在左窗格中，单击 " **VM 云**"。

3.  单击 "**注册 System Center Service Provider Foundation**，将 Windows Azure Pack 连接到 Service Provider Foundation 实例。 需要指定 Service Provider Foundation 的 URL，以及用户名和密码。

    ![注册 System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  完成后，你应该能够看到在 VMM 环境中设置的 VM 云。 请确保至少有一个 VM 云支持可用于 WAP 的受防护 Vm，然后再继续。

### <a name="create-a-plan-in-windows-azure-pack"></a>在 Windows Azure Pack 中创建计划

若要允许租户在 WAP 中创建 Vm，必须首先创建租户可以订阅的托管计划。 计划定义租户的允许 VM 云、模板、网络和计费实体。

1. 在门户的下方窗格中，单击 " **+ 新建**&gt;"**计划**@no__t**创建计划**。

2. 在向导的第一步中，选择计划的名称。 这是租户在订阅时将看到的名称。

3. 在第二步中，选择 "**虚拟机云**" 作为计划中提供的服务之一。

4. 跳过有关为计划选择任何加载项的步骤。

5. 单击 **"确定"** （复选标记）以创建计划。 尽管这会创建计划，但它尚不处于已配置状态。

   ![Windows Azure Pack 中的计划](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6. 若要开始配置计划，请单击其名称。

7. 在下一页上的 "**计划服务**" 下，单击 "**虚拟机云**"。 这会打开可在其中配置此计划的配额的页面。

8. 在 "**基本**" 下，选择要提供给租户的 VMM 管理服务器和虚拟机云。 可以提供受防护的 Vm 的云将显示在其名称旁 **（受支持屏蔽）** 。

9. 选择要在此计划中应用的配额。 （例如，对 CPU 核心和 RAM 使用量的限制）。 请确保选中 "**允许防护虚拟机**" 复选框。

   ![Windows Azure Pack 中的虚拟机云的设置](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10. 向下滚动到标题为 "**模板**" 的部分，然后选择要提供给租户的一个或多个模板。 你可以向租户提供防护和无屏蔽的模板，但必须提供受防护的模板，以便为租户提供对 VM 完整性及其机密的端到端保证。

11. 在 "**网络**" 部分中，为你的租户添加一个或多个网络。

12. 设置计划的任何其他设置或配额后，单击底部的 "**保存**"。

13. 在屏幕的左上方，单击箭头以返回到 "**计划**" 页。

14. 在屏幕底部，将计划从 "**专用**" 更改为 "**公共**"，以便租户可以订阅该计划。

    ![更改 Windows Azure Pack 中计划的访问权限](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    此时，已配置 Windows Azure Pack，租户将能够订阅刚创建的计划，并部署受防护的 Vm。 有关租户需要完成的其他步骤，请参阅[租户的受防护的 vm-通过使用 Windows Azure Pack 部署受防护的 vm](guarded-fabric-shielded-vm-windows-azure-pack.md)。

## <a name="see-also"></a>请参阅

- [受保护的主机和受防护的 Vm 的托管服务提供商配置步骤](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的结构和受防护的 VM](guarded-fabric-and-shielded-vms-top-node.md)
