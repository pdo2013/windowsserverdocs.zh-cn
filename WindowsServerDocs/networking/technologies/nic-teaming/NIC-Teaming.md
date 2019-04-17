---
title: NIC 组合
description: 本主题提供 Windows Server 2016 中的网络接口卡 (NIC) 分组的概述。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 142f56153187368effdb802c0c1b50359fffc36a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming"></a>NIC 组合

>适用于：Windows Server（半年通道），Windows Server 2016

本主题提供 Windows Server 2016 中的网络接口卡 (NIC) 分组的概述。

> [!NOTE]  
> 本主题中，除了 NIC 组合以下内容才可用。  
>   
> - [NIC 分组在虚拟机和 #40;虚拟机的功能和 #41;](nict-vms.md)
> - [NIC 组合和虚拟本地区域网络 & #40;Vlan & #41;](nict-and-vlans.md)
> - [NIC 分组 MAC 地址使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md)
> - [故障排除 NIC 组合](Troubleshooting-NIC-Teaming.md) 
> - [在主计算机或 VM 中创建新的 NIC 团队](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)
> - [NIC 组合 (NetLBFO) Cmdlet 的 Windows PowerShell](https://technet.microsoft.com/library/jj130849.aspx)
> - TechNet 库下载：[Windows Server 2016 NIC 和切换嵌入成组的用户指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)
  
## <a name="bkmk_over"></a>NIC 组合概述  
NIC 组合允许你在一个和三十两个物理以太网网络适配器一个或多个软件基于虚拟网络适配器插入之间进行分组。 这些虚拟网络适配器提供更快的性能以及故障功能的驱动器发生故障时的网络适配器。  
  
NIC 团队成员网络适配器必须所有将在相同的物理主计算机，要放置在团队中安装。  
  
> [!NOTE]  
> NIC 团队包含只有一个网络适配器无法提供负载平衡和故障转移;但是一个网络适配器，您可以使用 NIC 组合的分隔网络通信时还会使用虚拟本地网络 (Vlan)。  
  
插入 NIC 团队配置网络适配器时，则说明连接到 NIC 分组解决方案常见核心，然后呈现一个或多个虚拟适配器（也称为团队 Nic [tNICs] 或团队接口）的操作系统。 Windows Server 2016 支持 32 个团队接口每个团队。 有各种算法分配之间 Nic 站交通（加载）的支持。  
  
下图显示了使用多个 tNICs NIC 团队。  
  
![使用多个 tNICs NIC 团队](../../media/NIC-Teaming/nict_overview.jpg)  
  
此外，你可以连接成组的 Nic 到同一个交换机或不同的开关。 如果连接到不同的交换机 Nic，这两个开关必须相同子网。  
  
## <a name="bkmk_avail"></a>NIC 组合可用性  
NIC 组合是适用于所有版本的 Windows Server 2016。 此外，你可以使用 Windows PowerShell 命令、远程桌面和远程服务器管理工具 NIC 组合从计算机管理工具受支持的客户端操作系统运行。  
  
## <a name="bkmk_nics"></a>对于 NIC 组合支持和不支持 Nic  
你可以使用已通过在 Windows Server 2016 NIC 团队中的 Windows 硬件资格认定和徽标的测试（WHQL 测试）任何以太网卡。  
  
不能以下 Nic 放置在 NIC 团队。  
  
-   作为 Nic 公开主机分区中 Hyper-V 虚拟交换机用来端口 Hyper-V 虚拟网络适配器。  
  
    > [!IMPORTANT]  
    > 公开主机分区 (vNICs) 中的虚拟 Nic 尚未必须置于团队 Hyper-V。 中的任何配置或组合不支持 vNICs 主机分区内的团队合作。 如果网络失败出现，尝试到团队 vNICs 可能会导致完全丧失通信。  
  
-   内核调试的网络适配器 (KDNIC)。  
  
-   所用的网络启动的 Nic。  
  
-   使用以太网，如 WWAN、WLAN/Wi-fi、蓝牙和无限带宽，包括 Internet 协议通过无限带宽 (IPoIB) Nic 以外的技术的 Nic。  
  
## <a name="bkmk_compat"></a>NIC 组合兼容性  
NIC 组合是与 Windows Server 2016 中包括以下产品的所有网络的技术兼容。  
  
-   **单根 I/O 虚拟化 (SR IOV)**。 对于 SR-IOV 数据直接与 NIC 提供不必经过网络堆栈（在主机操作系统，就而言虚拟化）。 因此，不能 NIC 团队检查或数据重定向到另一台团队中的路径。  
  
-   **本机主机质量服务 (QoS)**。 当上设置 QoS 策略本机或主机系统和这些策略调用最低带宽限制，NIC 团队的整体吞吐量将小于不在地方带宽策略的情况下，将会。  
  
-   **TCP 烟囱**。 TCP 烟囱不支持使用 NIC 组合因为 TCP 烟囱减轻直接与 NIC 整个网络的堆栈  
  
-   **802.1 x 身份验证**。 802.1 不应 NIC 组合使用使用 X 身份验证。 不允许某些开关 802.1 X 身份验证和 NIC 组合同一端口配置。  
  
若要了解如何使用 Hyper-V 主机运行虚拟机 (Vm) 中的 NIC 组合，请参阅[NIC 组合在虚拟机和 #40;虚拟机的功能和 #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md).  
  
## <a name="bkmk_vmq"></a>NIC 组合和虚拟机队列 (VMQs)  
VMQ 和 NIC 组合搭配;当已启用 Hyper-V 应启用 VMQ。 根据我们切换配置模式和加载 distribution 算法，NIC 组合将呈现 VMQ 功能 Hyper-V 切换到显示可用队列团队（分钟队列模式）中的任何适配器支持的最小的版本号队列大量或队列提供的总数量为跨所有团队成员（Sum 的队列模式）。  
  
特别是，如果团队处于切换独立型分组模式和加载 Distribution 设置为 Hyper-V 端口模式或动态模式，然后队列报告数是可从（Sum 的队列模式;）的团队成员所有队列的总和否则队列报告数是小数队列受任何（分钟队列模式）团队的成员。  
  
下面介绍了原因：  
  
-   切换独立团队位于 Hyper-V 端口模式或动态模式时 Hyper-V 切换端口 (VM) 的入站的交通将始终到达上的同一团队的成员。 主机可以预测/控制哪些成员将收到交通特定 vm，可以更多有关特定团队成员上分配哪些 VMQ 队列认真 NIC 组合。 NIC 组合，处理 Hyper-V 切换，将设置一个团队成员上 VM VMQ 和知道站的交通将触及该队列。  
  
-   当团队处于（静态组队或 LACP 分组），任何切换依赖模式团队连接到的切换控件的入站的通信 distribution。 主机 NIC 组合软件无法预测哪些团队成员将获得的入站的交通 VM 和可能该开关分配流量的 VM 跨所有团队的成员。 当的结果工作了 Hyper-V 开关，NIC 组合软件用于在每个团队的成员 VM 程序队列，而不仅仅是一个团队成员。  
  
-   当团队切换独立型模式中，并使用地址哈希算法负载 distribution 时的入站的交通将始终会个 NIC（主要团队成员）-只需团队成员对所有上。 由于其他团队成员不处理它们获取编程方式设置与的入站通信相同队列作为主的成员，以便如果主成员失败任何其他团队成员可用于买传入的通信和队列不变。  
  
大多数 Nic 有可用于收到一侧缩放 (RSS) 或 VMQ，但不是能同时在同一时间的队列。 某些 VMQ 设置起来 RSS 队列的设置，但都非常设置，具体取决于哪些功能 RSS 和 VMQ 使用当前正在使用的通用队列。 每个 NIC 已在其中有高级属性，值为 * RssBaseProcNumber 和 \*MaxRssProcessors。 下面是几个提供更好的系统性能的 VMQ 设置。  
  
-   最好每个 NIC 应有 * RssBaseProcNumber 将设置为即使数字，大于或等同于两（2)。 这是因为第一个物理处理器，Core 0（0 和 1 逻辑处理器），通常完成大部分系统处理以便网络处理应 steered 离开该物理处理器。 （某些计算机体系结构没有每个物理处理器的两个逻辑处理器因此此类机基本处理器应小于 1。 如果不确定假定您的主机已使用 2 的逻辑处理器每个物理处理器体系结构。）  
  
-   如果团队 Sum 的队列型团队成员的处理器应该，范围实用，不重叠到。 例如，在带有 2 10Gbps Nic 的团队的 4 核心主机（8 逻辑处理器），你可以设置使用 2 的基本处理器以及如何使用 4 核心; 的第一个第二个会设置为使用基本处理器 6，并使用 2 个核心。  
  
-   如果团队分钟队列型团队成员使用处理器集必须相同。  
  
## <a name="bkmk_hnv"></a>NIC 组合和 Hyper-V 网络虚拟化 (HNV)  
NIC 组合是完全 Hyper-V 网络虚拟化 (HNV) 兼容。  HNV 管理系统提供给允许 NIC 组合以分散 HNV 交通进行了优化的方式负载 NIC 组合驱动程序的信息。  
  
## <a name="bkmk_live"></a>NIC 组合和实时迁移  
NIC 组合在虚拟机的功能不会影响实时迁移。 相同的规则 NIC 组合配置 VM 中存在实时迁移。  
  
## <a name="see-also"></a>请参阅  
[NIC 分组在虚拟机和 #40;虚拟机的功能和 #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md)  
  


