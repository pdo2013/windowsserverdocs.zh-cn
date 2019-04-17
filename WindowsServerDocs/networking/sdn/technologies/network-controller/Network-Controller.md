---
title: 网络控制器
description: 本主题提供 Windows Server 2016 中的网络控制器的概述。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: acb9abbd716e9930fb01431e7004abb72a7da10c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller"></a>网络控制器

>适用于：Windows Server（半年通道），Windows Server 2016

新的 Windows Server 2016，网络控制器提供的集中、可编程点自动化管理、配置，监视器和虚拟疑难解答和您的数据中心中的物理网络基础结构。 

使用网络控制器，你可以自动网络基础结构，而不是执行手动配置的网络的设备和服务的配置。

> [!NOTE]
> 本主题中，除了以下网络控制器文档才可用。
> - [网络控制器高可用性](network-controller-high-availability.md)
> - [安装和部署网络控制器的准备工作要求](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [部署使用 Windows PowerShell 网络控制器](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [安装网络控制器服务器角色使用服务器管理器](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [后 Post-Deployment 步骤网络控制器](post-deploy-steps-nc.md)
> - [网络控制器 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>网络控制器概述

网络控制器是高度可用和可扩展服务器角色，并提供一个应用程序编程接口 \(API\) 允许网络控制器通信网络，以及一个允许你与网络控制器通信的第二个 API。

你可以将网络控制器域和非域环境中部署。 在域环境网络控制器验证身份的用户和网络设备使用 Kerberos;你必须非域环境中部署身份验证的证书。

>[!IMPORTANT]
>不要部署物理主机上的了网络控制器服务器角色。 若要部署网络控制器，必须安装网络控制器服务器角色 Hyper-V 虚拟机上 \(VM\) Hyper-V 主机上安装。 通过添加到网络控制器使用 Windows PowerShell 命令主机上安装了网络控制器在虚拟机上三个不同的 Hyper\ V 主机后，必须启用软件定义网络 \(SDN\) Hyper\ V 主机**新建 NetworkControllerServer**。 通过执行此操作，你启用 SDN 软件负载平衡函数。 有关详细信息，请参阅[新建 NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

网络控制器使用 Southbound API 与网络设备、服务和组件进行通信。 与 Southbound API、网络控制器可以发现网络设备、检测服务配置中，并收集关于网络所需的信息的所有。 此外，Southbound API 提供网络控制器小路以信息发送到的网络的基础结构，如你所做的配置更改。

网络控制器 Northbound API 为您提供了从网络控制器收集网络信息并使用它来监视器和配置网络的功能。

网络控制器 Northbound API 允许你配置，监视器，疑难解答，请使用与图形用户界面，如系统中心虚拟机管理器中的 Windows PowerShell、具像状态传输 \(REST\) API，或管理应用程序部署网络上的新设备。

>[!NOTE]
>作为其余界面实现网络控制器 Northbound API。

可以通过使用管理应用程序，如 System Center 虚拟机 Manager \(SCVMM\) 和 System Center Operations Manager \(SCOM\)，因为网络控制器，您可以配置、监视器、计划、管理你的数据中心网络通过网络控制器和疑难解答下其控件的网络基础结构。

使用 Windows PowerShell、REST API 或管理应用程序，你可以通过网络控制器管理以下物理和虚拟网络基础：

- Hyper-V 虚拟机的功能和虚拟交换机

- Datacenter 防火墙

- 远程访问服务 \(RAS\) Multitenant 网关、虚拟网关、和网关池

- 软件负载平衡

下图中，在管理员直接与网络控制器使用交互管理工具。 网络控制器提供关于网络的基础结构，包括虚拟和物理基础结构，对管理工具的信息，并使根据管理员操作时使用该工具的配置更改。  

![网络控制器概述](../../../media/Network-Controller/NetController_overview.png)  

如果你正在测试实验环境中部署网络控制器，你可以 Hyper-V 虚拟机上运行网络控制器服务器角色 \(VM\) Hyper-V 主机上安装。

提供更大数据中心中的高应用，你可以通过使用三个或多个 Hyper-V 主机安装的三个 Vm 部署群集。 有关详细信息，请参阅[网络控制器高可用性](network-controller-high-availability.md)。

## <a name="bkmk_features"></a>网络控制器功能

以下网络控制器功能允许您将配置并管理虚拟和物理网络的设备和服务。  
  
-   [防火墙管理](#bkmk_firewall)  
  
-   [软件负载平衡管理](#bkmk_slb)  
  
-   [虚拟网络管理](#bkmk_virtual)  
  
-   [RAS 网关管理](#bkmk_gateway)

>[!IMPORTANT]
>网络控制器备份和还原不当前适用于 Windows Server 2016。
  
### <a name="bkmk_firewall"></a>防火墙管理

此网络控制器功能可以配置和东月西和北美/南网络通信，您的数据中心中管理允许拒绝防火墙访问控制你的工作负载 Vm 规则。 防火墙规则检测的工作负载虚拟机的功能，该 vSwitch 端口中，因此它们分布在你的工作负载数据中心中。 使用 Northbound API，你可以定义防火墙规则从工作负载 VM 传入和传出交通。 你还可以配置每个防火墙规则记录交通已允许使用或规则拒绝。  

有关详细信息，请参阅[Datacenter 防火墙概述](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。

### <a name="bkmk_slb"></a>软件负载平衡管理

此网络控制器功能将允许你启用主持提供高可用性和可扩展性相同的工作负荷多个服务器。  
  
有关详细信息，请参阅[软件负载平衡 & #40;SLB & #41;对于 SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
### <a name="bkmk_virtual"></a>虚拟网络管理

此网络控制器功能将允许你以将其部署和配置 Hyper-V 网络虚拟化，包括虚拟交换机用来 Hyper-V 和个别虚拟机的功能的虚拟网络适配器和存储和分发虚拟网络策略。

网络控制器支持网络虚拟化泛型路由封装 (NVGRE) 和虚拟可扩展本地网络 (VXLAN)。

### <a name="bkmk_gateway"></a>RAS 网关管理

此网络控制器功能允许您将配置，和管理虚拟机 (Vm) 的成员 RAS 网关池，向你租户提供网关服务部署。 网络控制器允许您自动部署网关以下功能的运行 RAS 网关虚拟机的功能：

> [!NOTE]
> 在系统中心虚拟机管理器 RAS 网关称作 Windows Server 网关。

- 添加和删除从群集网关虚拟机的功能，并指定备份所需的级别。

- 站点的虚拟专用网络 (VPN) 网关远程租户网络和使用 IPsec 你 datacenter 之间的连接。

- 站点的 VPN 网关之间的连接性远程租户网络并使用通用路由封装 (GRE) 你数据中心。

- 层 3 转移功能。

- 边框网关协议 (BGP) 路由，这允许您管理传送网络租户的 VM 网络和他们远程访问的站点之间的交通。

网络控制器可以单独网关放置的租户不同的连接。 可以使用单个公共 IP 所有网关连接或不同的公用 IPs 子集的连接。 所有网关配置和可用于审核和疑难解答的状态更改网络控制器日志。

BGP 的详细信息，请参阅[边框网关协议 & #40;BGP & #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

RAS 网关的详细信息，请参阅[SDN RAS 网关](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。

## <a name="network-controller-deployment-options"></a>网络控制器部署选项

若要使用 System Center 虚拟机 Manager \(VMM\) 部署网络控制器，请参阅[设置 VMM 结构中 SDN 网络控制器](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

若要使用脚本网络控制器部署时，请参阅[部署软件定义的网络的基础结构使用脚本](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要使用 Windows PowerShell 网络控制器部署时，请参阅[使用 Windows PowerShell 部署网络控制器](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
