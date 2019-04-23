---
title: Azure 虚拟网络集成
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6193d2e2a2c0b85e13d4e9474264d2581bea88fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879358"
---
#<a name="azure-virtual-network-integration"></a>Azure 虚拟网络集成

>适用于：Windows Server 2016 Essentials

组织进行云计算其方法，很少会将它们一次将其资源的所有移动 100%但而不是采用一种方法，其中一些资源是在云中，一些是仍在本地。 此混合方法可方便组织不仅将一些计算资源移动到云，但也使他们能够提升其 IT 基础结构，而无需购买新硬件。

实现对计算此混合方法，这两个位置中的资源无缝地相互通信时必需。 Azure 虚拟网络是 Azure 服务，使组织能够创建点对点 (P2P) 或站点到站点 (S2S) 虚拟专用网络以便让像是运行在 Azure （例如虚拟机和存储） 中查找的资源在应用程序和资源的无缝访问本地网络。

Azure 虚拟网络的配置可能很复杂。 Windows Server Essentials 2016 可以轻松地配置 Azure 虚拟网络通过一个简单的向导，可帮助你选择最合适的默认值为联网环境。 新的 Azure 虚拟网络集成任务中的屏幕截图所示，已添加到要引入 Azure 虚拟网络，以及提供的快速链接来启动集成的 Windows Essentials 仪表板的 Microsoft 云服务部分.

![Windows Server Essentials 仪表板的主页上显示开始选项卡屏幕截图。 在开始选项卡上，已选择服务部分，和仪表板的 Microsoft 云服务集成下方，指示当前已禁用 Azure 虚拟网络。](media/azure-virtual-network-1.PNG)

当您单击**现在将集成**上面的屏幕截图中的 Azure 虚拟网络中的链接将显示一个对话框，要求你登录到 Microsoft Azure 帐户。 如果还没有 Microsoft Azure 帐户，将拥有一个在此屏幕上，将重定向到 Azure 帐户注册门户注册选项：

![显示集成与 Azure 虚拟网络向导的登录到 Microsoft Azure 页屏幕截图。](media/azure-virtual-network-2.PNG)

一旦登录到 Azure，你将看到用于选择他们想要将与 Azure 虚拟相关联的订阅的选项网络服务：

![显示与 Azure 虚拟网络向导集成的我的 Microsoft Azure 订阅页屏幕截图。](media/azure-virtual-network-3.PNG)

一旦您已选择你想要使用 Azure 虚拟网络的 Azure 订阅将显示的选项创建新的 Azure 虚拟网络，或如果其中一个已在此订阅下的安装程序，下拉列表框将显示可用。 此外将选择 Azure 虚拟网络将用来识别你的本地网络中的资源的本地网络的名称。 最后，将选择要在其中托管其 Azure 虚拟网络的 Azure 区域。 选择以物理方式最接近你的本地网络的位置是通常最适合优化带宽速度与可能在其 Azure 服务中托管的资源进行通信：

![显示集成与 Azure 虚拟网络向导的设置启动 Azure 虚拟网络页屏幕截图。](media/azure-virtual-network-4.PNG)

集成过程的最后一步是设置将用于 S2S VPN 连接的 VPN 设备。 由于大多数小型企业在其环境中有仅几个服务器，并且缺少 IT 人员若要正确配置 VPN 路由器连接到 Microsoft Azure，则默认选择将设置为 VPN 服务器的资源的 Windows Server Essentials 服务器在你的本地网络将连接到要访问 Azure 虚拟网络中的资源。 但是，如果将而不是使用另一台服务器在您的环境中为 VPN 服务器，或者您更愿意使用 VPN 路由器，您可以选择这些选项。

由于路由器类型和模型中的变体，而 Windows Server Essentials 不会尝试自动配置 VPN 路由器。 此集成向导中选择 VPN 路由器仅通知适当的路由配置所需在 Azure 中连接的设备类型的 Azure 虚拟网络。

正在完成集成向导，在新选项卡会显示在 Azure 虚拟网络的 Windows Server Essentials 仪表板：

![显示 Windows Server Essentials 仪表板的 Azure VNet 页的屏幕截图。 Azure 虚拟网络选项卡处于选中状态，并将状态显示为配置。](media/azure-virtual-network-5.PNG)

>!请注意在云中的 Azure 虚拟网络的配置可能需要很长时间，向上到 30 分钟内完成。 在此期间，配置的状态将会出现在 Azure 虚拟网络状态的仪表板页。

Azure 虚拟网络的配置完成后，状态将更改为已连接，并显示如数据输入/输出、 网关 IP 地址、 本地 IP 地址和帐户详细信息在 Azure 虚拟网络的详细信息：

![显示 Windows Server Essentials 仪表板的 Azure VNet 页的屏幕截图。 Azure 虚拟网络选项卡处于选中状态，并将状态显示为已连接，并在虚拟网络的详细信息显示在此状态信息。](media/azure-virtual-network-6.PNG)

在仪表板右侧的任务窗格是你可能需要与 Azure 虚拟网络的各种任务。

-   **断开连接从 Azure VNET**设置 Azure 虚拟网络是免费的但连接到在本地和 Azure 中的其他 Vnet 的 VPN 网关的费用。 断开与 Azure VNET 的连接将停止所有计费。

-   **通过 VPN 设备切换**的事件中你想要从 VPN 服务器更改为 VPN 路由器，此任务便可以进行切换并通知 Azure VNET。

-   **配置 Azure VNET**此任务，您可以更改的将它们到 Azure 门户配置页面重定向为 Azure VNET 的 Azure VNET 的高级的配置选项。

-   **刷新状态**刷新状态页上，更新包括输入/输出数据的 Azure VNET 的连接状态。

-   **禁用 Azure VNET 集成**断开连接 Azure VNET，并从 Windows Server Essentials 仪表板中删除的集成。 请注意这不会删除 Azure VNET，如果你想要更高版本使用仪表板重新集成 Azure VNET，设置仍将保留在 Azure 中。

-   **了解有关 Azure VNET 的详细信息** [ https://azure.microsoft.com/services/virtual-network/ ](https://azure.microsoft.com/services/virtual-network/)。

<a name="see-also"></a>请参阅
--------
[开始使用 Windows Server Essentials](get-started.md)
