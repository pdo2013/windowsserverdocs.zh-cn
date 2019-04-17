---
title: "Azure 虚拟网络的集成"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 21057359967c73d8d9fc434694b8f203ad5c1f6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
#<a name="azure-virtual-network-integration"></a>Azure 虚拟网络的集成

>适用于：Windows Server 2016 软件包

当组织进行计算云他们方式时，很少将它们移动其所有资源 100%一次但而采用的方法，其中一些资源在云中以及一些仍在本地。 此混合方法很容易不仅组织移动到云的某些计算资源，但还允许他们增长 IT 基础结构，而无需获取新的硬件。

在实现此混合不同的推送方案计算时，这两个位置中的资源无缝方式以与另一个通信，才能。 Azure 虚拟网络都是 Azure 服务，可使企业创建点到点 (P2P) 或站点的 (S2S) 虚拟专用网络，以便在 Azure （如虚拟机和存储） 查找一样的应用程序和资源的无缝访问本地网络上运行的资源。

Azure 虚拟网络配置可能很复杂。 您可以轻松配置 Azure 虚拟网络可帮助你选择最适合用于你的网络环境默认值的简单向导通过与 Windows Server Essentials 2016。 下面的屏幕截图中所示，新的 Azure 虚拟网络集成任务已添加到 Windows 软件包仪表板引入 Azure 虚拟网络以及提供快速链接发起集成的 Microsoft 云服务部分。

![在主页上的 Windows Server Essentials 仪表板中显示的开始选项卡的屏幕截图。 在开始选项卡上，已选择服务部分，，并且在 Microsoft 云服务集成指示仪表板 Azure 虚拟网络当前已禁用。](media/azure-virtual-network-1.PNG)

在你单击时**现在集成**链接上述、 屏幕截图在 Azure 虚拟网络要求你登录到你的 Microsoft Azure 帐户，将显示一个对话框。 如果你没有 Microsoft Azure 帐户，你将有注册到此屏幕，它将重定向到 Azure 帐户门户，注册上一个选项：

![显示的集成使用 Azure 虚拟网络向导登录中向 Microsoft Azure 页面的屏幕截图。](media/azure-virtual-network-2.PNG)

后登录中到 Azure，你将看到的选项来选择他们想要使用 Azure 虚拟关联的订阅网络服务：

![屏幕截图显示了使用 Azure 虚拟网络向导集成的我的 Microsoft Azure 订阅页。](media/azure-virtual-network-3.PNG)

当你选择哪种 Azure 订阅，你想要使用 Azure 虚拟网络，你将看到的选项来创建新的 Azure 虚拟网络或已一个已在此订阅的设置，如果下拉框将显示可用。 你还将选择 Azure 虚拟网络将用于识别您的本地网络资源本地网络的名称。 最后，你会选择想要为投放其 Azure 虚拟网络哪个 Azure 地区。 选择某一位置的物理离本地网络最好通常优化带宽速度可能托管其 Azure 服务中的资源与通信：

![显示设置向上 Azure 虚拟网络页面集成使用 Azure 虚拟网络向导中的屏幕截图。](media/azure-virtual-network-4.PNG)

集成进程的最后一步是用于 S2S VPN 连接 VPN 设备的设置。 由于最小型企业中他们的环境中有仅少数服务器，并且缺少 IT 人员正确配置连接到 Microsoft Azure VPN 路由器，默认选择要设置为在你的本地网络资源才能访问 Azure 虚拟网络中的资源将连接到 VPN 服务器的 Windows Server Essentials 服务器。 但是，如果希望使用另一台服务器在您的环境中为 VPN 服务器，或者你希望使用 VPN 路由器，你可以选择这些选项。

由于的变体路由器类型和型号中，Windows Server Essentials 不会尝试自动配置 VPN 路由器。 选择此集成向导中的 VPN 路由器仅通知 Azure 虚拟网络的相应路由配置必要 Azure 在连接的设备类型。

在完成集成向导，请将为 Azure 虚拟网络 Windows Server Essentials 仪表板中可见一个新选项卡：

![显示 Windows Server Essentials 仪表板 Azure VNet 页面的屏幕截图。 Azure 虚拟网络选项卡上选择，并显示为配置的状态。](media/azure-virtual-network-5.PNG)

>!注意完成配置云中的 Azure 虚拟网络可能需要很长时间，向上到 30 分钟。 在此期间，将显示在 Azure 虚拟网络状态页仪表板中配置的状态。

Azure 虚拟网络配置完毕后，将切换到已连接的状态并将其显示 Azure 虚拟网络等数据/出、 网关 IP 地址、 本地 IP 地址和帐户的详细信息的详细信息：

![显示 Windows Server Essentials 仪表板 Azure VNet 页面的屏幕截图。 Azure 虚拟网络选项卡上选择，并显示为已连接的状态，并在此状态信息显示虚拟网络的详细信息。](media/azure-virtual-network-6.PNG)

在仪表板右侧的任务窗格都是你可以使用 Azure 虚拟网络需要的各种任务。

-   **Azure VNET 从断开连接**设置 Azure 虚拟网络是免费的但没有连接到本地以及其他 VNETs 在 Azure VPN 关收取费用。 断开 Azure VNET 停止所有计费。

-   **切换通过 VPN 设备**在你想要从 VPN 服务器更改为 VPN 路由器，此任务将允许你进行切换并通知 Azure VNET。

-   **配置 Azure VNET**此任务中，可通过将其到 Azure 门户配置页重定向的 Azure VNET 更改 Azure VNET 的高级的配置选项。

-   **刷新状态**刷新状态页面，更新中/出包括数据 Azure VNET 的连接状态。

-   **禁用 Azure VNET 集成**断开连接 Azure VNET 和从 Windows Server Essentials 仪表板中删除集成。 请注意，这不会删除 Azure VNET，因此如果你想要在以后重新集成 Azure VNET 仪表板，设置仍将保留在 Azure。

-   **了解更多有关 Azure VNET**[https://azure.microsoft.com/en-us/services/virtual-network/](https://azure.microsoft.com/en-us/services/virtual-network/)。

<a name="see-also"></a>请参阅
--------
[Windows Server Essentials 入门](get-started.md)
