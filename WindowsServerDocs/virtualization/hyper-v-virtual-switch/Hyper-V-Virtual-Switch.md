---
title: Hyper-V 虚拟交换机
description: 本主题概述了 Windows Server 2016 中的 HYPER-V 虚拟交换机。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17962b9bbb90a5ea1b6a4a303c64d72f74be37b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860918"
---
# <a name="hyper-v-virtual-switch"></a>Hyper-V 虚拟交换机

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题提供的 HYPER-V 虚拟交换机，提供了功能将虚拟机连接概述\(Vm\)到网络外部的超\-V 主机，包括贵组织的 intranet和 Internet。 

您还可以连接到虚拟网络的服务器上的正在运行超\-V 部署软件定义的网络时\(SDN\)。

> [!NOTE]  
> 除了本主题提供了以下 HYPER-V 虚拟交换机文档。  
>   
> - [管理的 HYPER-V 虚拟交换机](Manage-Hyper-V-Virtual-Switch.md) 
> - [远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](RDMA-and-Switch-Embedded-Teaming.md)
> - [在 Windows PowerShell 中的网络交换机组 Cmdlet](https://technet.microsoft.com/library/jj553812.aspx)
> - [什么是 VMM 2016 中的新增功能](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [设置 VMM 网络构造](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [使用 VMM 2012 创建网络](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-V：配置 Vlan 和 VLAN 标记](https://social.technet.microsoft.com/wiki/contents/articles/1306.hyper-v-configure-vlans-and-vlan-tagging.aspx)  
> - [Hyper-V：如果它必需的第三方扩展，应启用 WFP 虚拟交换机扩展](https://social.technet.microsoft.com/wiki/contents/articles/13071.hyper-v-the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions.aspx)
>
> 有关其他网络技术的详细信息，请参阅[Windows Server 2016 中的网络](https://docs.microsoft.com/windows-server/networking/networking)。
  
超\-V 虚拟交换机是基于软件的第 2 层以太网网络交换机，可在超\-V 管理器时安装超\-V 服务器角色。

HYPER-V 虚拟交换机包括以编程方式管理和扩展功能，若要将 Vm 连接到虚拟网络和物理网络。 此外，Hyper-V 虚拟交换机提供了安全、隔离和服务级别的策略执行。  
  
> [!NOTE]  
> HYPER-V 虚拟交换机仅支持以太网，不支持其他任何有线局域网 (LAN) 技术（如 Infiniband 和光纤通道）。  
  
HYPER-V 虚拟交换机包括租户隔离功能、 流量整形、 恶意虚拟机防护、 以及简化的故障排除。 

对网络设备接口规范的内置支持\(NDIS\)筛选器驱动程序和 Windows 筛选平台\(WFP\)标注驱动程序的 HYPER-V 虚拟交换机使独立软件供应商\(Isv\)创建可扩展插件，称为虚拟交换机扩展，可提供增强的网络和安全功能。 你添加到 Hyper-V 虚拟交换机的虚拟交换机扩展列在 Hyper-V 管理器的虚拟交换机管理器功能中。
  
在下图中，VM 已连接到在 HYPER-V 虚拟交换机通过交换机端口的虚拟 NIC。  
  
![虚拟交换机连接](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
HYPER-V 虚拟交换机功能为您提供更多选项执行租户隔离、 整形和控制网络流量，并采用保护测量恶意虚拟机。

>[!NOTE]
> Windows Server 2016 中具有虚拟 NIC 的 VM 准确地显示的最大吞吐量的虚拟 nic。 若要查看中的虚拟 NIC 速度**网络连接**，右键单击所需的虚拟 NIC 图标，然后单击**状态**。 虚拟 NIC**状态**对话框随即打开。 在中**连接**的值**速度**匹配服务器中安装的物理 NIC 的速度。
  
## <a name="bkmk_apps"></a>用于 HYPER-V 虚拟交换机

以下是一些用例方案中的为 HYPER-V 虚拟交换机。

**显示统计信息**:受托管云提供商的开发者实现了显示 Hyper-V 虚拟交换机当前状态的管理程序包。 该管理程序包可以使用 WMI 来查询交换机范围的当前能力、配置设置和单个端口网络统计。 然后会显示交换机的状态，为管理员提供交换机状态的快速视图。  
  
**资源追踪**:托管公司销售托管服务，销售价格根据成员身份级别确定。 各种成员级别包括不同的网络性能级别。 管理员以平衡网络可用性的方式分配资源，以符合服务级别协议 (SLA)。 管理员以编程方式跟踪信息，例如当前使用情况的已分配带宽和虚拟机 (VM) 分配虚拟机队列 (VMQ) 或 IOV 通道数。 除了为了进行复式记帐追踪或资源而分配的 VM 资源，同一个程序还周期性地记录使用中的资源。  
  
**管理交换机扩展的顺序**:一家企业已经在他们的 Hyper-V 主机上安装了扩展，用于监视流量和报告入侵检测。 在维护时，某些扩展可能被更新了，导致扩展的顺序变更。 运行了一个简单的脚本程序来记录更新后的扩展。  
  
**转发扩展管理 VLAN ID**:一个主要交换机公司正在构建可应用于所有网络策略的转发扩展。 受管理的一个元素是虚拟局域网 (VLAN) ID。 虚拟交换机将 VLAN 的控制移交给转发扩展。 交换机公司的安装以编程方式调用 Windows Management Instrumentation (WMI) 应用程序编程接口 (API) 的启用透明度，通知 HYPER-V 虚拟交换机通过并且对 VLAN 标记不采取任何操作。  
  
## <a name="bkmk_func"></a>HYPER-V 虚拟交换机功能
 
Hyper-V 虚拟交换机包含的主要功能包括：  
  
-   **ARP/ND 病毒 （欺骗） 防护**:针对使用地址解析协议 (ARP) 欺诈来窃取其他 VM 的 IP 地址的恶意 VM 提供防护。 针对使用邻居发现 (ND) 欺诈的 IPv6 进行的攻击提供防护。  
  
-   **DHCP 防护保护**:针对伪装成动态主机配置协议 (DHCP) 服务器以进行中间人攻击的 VM 提供防护。  
  
-   **端口 Acl**:提供基于媒体访问控制 (MAC) 或互联网协议 (IP) 地址/范围的流量过滤，让你可以设置虚拟网隔离。  
  
-   **VM 的 trunk 模式**:使管理员可以将特定的 VM 设置为虚拟设备，然后将来自各种 VLAN 的流量引导至该 VM。  
  
-   **网络流量监控**:使管理员可以查看遍历网络交换机的流量。  
  
-   **隔离的 （私有） VLAN**:使管理员可以隔离多个 VLAN 上的流量，以更方便地建立隔离的租户社区。  
  
以下是能够增强 Hyper-V 虚拟交换机可用性的一系列功能：  
  
-   **带宽限制和迸发支持**:最低带宽确保预留的带宽。 最大带宽超过一个 VM 能够消耗的带宽。  
  
-   **显式拥塞通知 (ECN) 标记支持**:ECN 标记，也称为数据中心 (DCTCP)，使物理交换机和操作系统可以调节流量，以便交换机缓存资源溢出，从而可以提高的流量吞吐量。  
  
-   **诊断**:使用诊断让事件和通过虚拟交换机的包的跟踪和监控更加方便。
