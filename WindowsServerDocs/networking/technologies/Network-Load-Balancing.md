---
title: 网络负载平衡
description: 本主题提供的 Windows Server 2016，该网络负载平衡 (NLB) 功能概述，并包含指向有关创建、配置和管理 NLB 群集其他指南。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b9fd39381316a8bcd06328e7aa75492ed99bc7f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-load-balancing"></a>网络负载平衡

>适用于：Windows Server（半年通道），Windows Server 2016

本主题提供的 Windows Server 2016，该网络负载平衡 \(NLB\) 功能概述，并包含指向有关创建、配置和管理 NLB 群集其他指南。

可以使用 NLB 作为一个虚拟群集管理两个或多个服务器。 NLB 增强了可用性和可扩展性 Internet 服务器等这些 web FTP 上使用的应用程序、防火墙，代理，虚拟专用网络 \(VPN\)，以及其他 mission\ 关键的服务器。   

>[!NOTE]
>Windows Server 2016 作为软件定义网络 \(SDN\) 基础结构的组件包含新的 Azure 灵感软件负载平衡 \(SLB\)。 使用而不是 NLB SLB 如果你使用的 SDN，使用非 Windows 工作负载，需要站网络地址转换 \(NAT\)，或需要层 3 \(L3\) 或非 TCP 基于负载平衡。 你可以继续使用 Windows Server 2016 非 SDN 部署 NLB。 有关 SLB 的详细信息，请参阅[软件负载平衡 (SLB) 的 SDN](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。

网络负载平衡 \(NLB\) 功能分配跨多个服务器流量使用 TCP\/IP 网络协议。 通过集中到一个虚拟群集运行的应用的两个或多个计算机，NLB 提供的可靠性和 web 服务器的性能以及其他 mission\ 关键的服务器。  
  
服务器 NLB 群集中的被称为*主机*，并且每台主机运行一份的服务器应用程序。 NLB 跨群集中主机分发传入的客户端请求。 您可以配置每个主机处理加载。 你还可以添加到群集动态主机处理增加的负载。 NLB 也可以直接与指定的一个主机，称为的所有通信*默认主机*。  
  
NLB 使的所有计算机群集解决的问题的 IP 地址的同一组中，并为每台主机维护一套唯一专用的 IP 地址。 对于 load\ 平衡应用程序，主机将无法正常工作或离线时, 加载是自动重新分发仍在运行的计算机之间。 准备就绪后，离线状态下的计算机可以透明重新加入群集、重新负载，以便使的其他计算机中要处理更少的流量群集共享。  
  
## <a name="BKMK_APP"></a>实际应用  
NLB 是否有助于确保无状态的应用程序，如 web 服务器运行 Internet 信息服务 \(IIS\)，可以用降至最低停机，以及他们是否可缩放 \（通过作为加载 increases\ 添加更多服务器）。 以下部分介绍了如何 NLB 支持高可用性、可扩展性和可管理性的群集运行这些应用程序的服务器。  
  
### <a name="high-availability"></a>高可用性  
高可用性系统可靠提供服务用降至最低停机接受级别。 来可用性、NLB 包含可以自动 built\ 中功能：  
  
-   检测无法或离线的群集主机，然后恢复。  
  
-   添加或删除主机时，请平衡网络加载。  
  
-   恢复和 10 秒钟内重新分发工作负载。  
  
### <a name="scalability"></a>可扩展性  
可扩展性是以及计算机、服务或应用程序可增长需求提高性能的度量单位。 对于 NLB 群集可扩展性是逐步将一个或多个系统添加到现有群集群集整体加载超过其功能时的能力。 支持可扩展性，你可以执行 NLB 与以下操作：  
  
-   通过单个 TCP\/IP 服务 NLB 群集余额加载请求。  
  
-   在单个群集支持 32 个的计算机。  
  
-   余额服务器加载的多个请求 \（相同的客户端从或多个 clients\）跨群集中的多个主机。  
  
-   向添加主机 NLB 群集加载越大，而不会导致群集故障。  
  
-   从群集中删除主机时加载降低。  
  
-   启用高性能和低开销通过管道完全实现。 管道允许请求发送到 NLB 群集，而无需等待以前请求的响应。  
  
### <a name="manageability"></a>可管理性  
支持的可管理性，你可以执行下列操作 NLB 与：  
  
-   管理和配置通过 NLB 管理器的多个 NLB 群集并从一台计算机群集主机或[网络负载平衡 (NLB) Cmdlet 的 Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx)。
  
-   指定负载平衡使用端口管理规则单个 IP 端口或一组端口的行为。  
  
-   定义每个网站的其他端口规则。 如果你使用多个应用或网站 load\ 平衡服务器的同一组，规则端口根据目标虚拟 IP 地址 \(using virtual clusters\)。  

-   直接所有客户请求和一台主机使用可选、single\ 主机规则。 NLB 路线客户端将请求到运行特定应用程序的特定主机。  

-   阻止不需要的网络访问某些 IP 端口。  

-   启用群集主机控制切换端口流上的 Internet 组管理协议 \(IGMP\) 支持 \（其中传入网络数据包发送到 switch\ 上的所有端口）时多路广播模式在操作系统。  

-   启动、停止，并使用 Windows PowerShell 命令或脚本远程控制 NLB 操作。  

-   查看 Windows 事件日志，若要检查 NLB 事件。 NLB 记录的事件日志中的所有操作和群集的更改。  

## <a name="important-functionality"></a>重要的功能  
 
安装 NLB 作为标准 Windows Server 网络驱动程序组件。 其操作是透明到 TCP\/IP 网络堆栈。 下图显示在典型配置 NLB 和其他软件组件的关系。  
  
![网络负载平衡和其他软件组件](../media/NLB/nlb.jpg)  
  
以下是 NLB 的主要功能。  
  
- 不需要的硬件更改运行。  
  
- 提供了网络负载平衡工具将配置并从一台远程或本地计算机管理多个群集和的所有主机。  
  
- 使客户端无法访问在使用的单个的逻辑 Internet 名称和虚拟 IP 地址，也称为群集 IP 地址群集 \（保留为每个 computer\ 个别名称）。 NLB 这样多主服务器多个虚拟的 IP 地址。  
  
> [!NOTE]  
> 当部署作为虚拟群集虚拟机的功能时，NLB 不需要服务器多宿主具有多个虚拟的 IP 地址。  
  
- 使 NLB 绑定到多个网络适配器，可用于配置每台主机上的多个独立的群集。 多个网络适配器的支持，虚拟群集允许你配置了一个网络适配器上的多个群集与虚拟群集。  
  
- 不需要修改服务器应用，以便他们可以在 NLB 群集运行。  
  
- 可以配置为自动添加到群集如果该群集主机失败，随后是主机联机。 添加的主机可以开始处理客户端的新服务器请求。  
  
-   使你采取预防维护计算机离线，而不会影响其他主机上的群集操作。  
  
## <a name="BKMK_HARD"></a>硬件要求  
以下是运行 NLB 群集的硬件要求。  
  
-   群集中的所有主机必须都位于相同子网。  
  
-   无限制的数量上每个主机，网络适配器的上，并且其他的电话号码的适配器有不同的主机。  
  
-   在每个群集，所有网络适配器都必须多路广播或单址广播。 NLB 不支持多址广播和单单个群集内的址广播混合的环境。  
  
-   如果你使用的址，用于处理 client\ to\ 群集的通信的网络适配器必须支持更改其媒体访问控制 \(MAC\) 地址。  
  
## <a name="BKMK_SOFT"></a>软件要求  
以下是运行 NLB 群集的软件要求。  
  
-   唯一的 TCP\ IP 可在每台主机为其启用 NLB 适配器。 未添加任何其他协议 \ (例如，IPX\) 此适配器。  
  
-   必须静态服务器在群集的 IP 地址。  
  
> [!NOTE]  
> NLB 不支持动态主机配置协议 \(DHCP\)。 NLB 禁用上它配置每个接口 DHCP。  
  
## <a name="BKMK_INSTALL"></a>安装信息  
你可以通过使用适用于 NLB 服务器管理器或 Windows PowerShell 命令安装 NLB。

（可选）你还可以安装网络负载平衡工具将本地或远程 NLB 群集来管理。 这些工具包括网络负载平衡管理和 NLB Windows PowerShell 命令。

### <a name="installation-with-server-manager"></a>安装使用服务器管理器

在服务器管理器中，你可以使用添加角色和功能向导添加**网络负载平衡**功能。 完成向导后，安装 NLB，以及你不需要重启计算机。


### <a name="installation-with-windows-powershell"></a>使用 Windows PowerShell 安装  

若要使用的 Windows PowerShell 安装 NLB，需要在你想要安装 NLB 在计算机上在提升了 Windows PowerShell 提示符运行以下命令。

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
安装完成后，不重新启动计算机是必需的。

有关详细信息，请参阅[安装 WindowsFeature](https://technet.microsoft.com/library/jj205467.aspx)。

### <a name="network-load-balancing-manager"></a>网络负载平衡管理
若要打开网络负载平衡管理器中服务器管理器，请单击**工具**，然后单击**网络负载平衡管理**。
  
## <a name="BKMK_LINKS"></a>更多资源  
下表提供了有关 NLB 功能的其他信息的链接。  
  
|键入的内容|引用|  
|----------------|--------------|  
|部署|[网络负载平衡部署指南](https://technet.microsoft.com/library/cc754833(WS.10).aspx)& #124;[配置网络负载平衡终端服务](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|操作|[管理网络负载平衡群集](https://technet.microsoft.com/library/cc753954(WS.10).aspx)& #124;[设置网络负载平衡参数](https://technet.microsoft.com/library/cc731619(WS.10).aspx)& #124;[控制主机网络负载平衡群集上](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|故障排除|[解决网络负载平衡群集](https://technet.microsoft.com/library/cc732592(WS.10).aspx)& #124;[NLB 群集事件和错误](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|工具和设置|[网络负载平衡 Windows PowerShell cmdlet](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|社区资源|[高可用性 \(Clustering\) 论坛](https://go.microsoft.com/fwlink/p/?LinkId=230641)