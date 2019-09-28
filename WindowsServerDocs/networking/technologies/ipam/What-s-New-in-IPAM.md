---
title: IPAM 中的新增功能
description: 本主题介绍 Windows Server 2016 中新增或更改的 IP 地址管理（IPAM）功能。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fc19a58482df5dfbfb4ea324f317bbe1b27bf834
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405597"
---
# <a name="whats-new-in-ipam"></a>IPAM 中的新增功能

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍 Windows Server 2016 中新增或更改的 IP 地址管理（IPAM）功能。  
  
IPAM 为企业或云服务提供商（CSP）网络上的 IP 地址和 DNS 基础结构提供可高度自定义的管理和监视功能。 你可以使用 IPAM 监视、审核和管理运行动态主机配置协议（DHCP）和域名系统（DNS）的服务器。  
  
## <a name="BKMK_IPAM2012R2"></a>IPAM 服务器中的更新  
下面是适用于 Windows Server 2016 中的 IPAM 的新增功能和改进功能。  
  
|特性/功能|新功能或改进功能|描述|  
|--------------------------|-------------------|---------------|  
|[增强的 IP 地址管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|改进功能|在处理 IPv4/32 和 IPv6/128 子网以及查找 IP 地址块中的可用 IP 地址子网和范围时，会改进 IPAM 功能。|  
|[增强的 DNS 服务管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|新增|IPAM 支持 DNS 资源记录、条件转发器和 DNS 区域管理，适用于已加入域的 Active Directory 集成和支持文件的 DNS 服务器。|  
|[集成的 DNS、DHCP 和 IP 地址（DDI）管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|改进功能|启用了几种新体验和集成生命周期管理操作，例如，可视化所有与 IP 地址相关的 DNS 资源记录，基于 DNS 资源记录自动列出 IP 地址，以及 IP 地址生命周期管理用于 DNS 和 DHCP 操作。|  
|[多 Active Directory 林支持](#bkmk_ad)|新增|当安装 IPAM 的林与每个远程林之间存在双向信任关系时，可以使用 IPAM 管理多个 Active Directory 林的 DNS 服务器和 DHCP 服务器。|  
|[清除利用率数据](#bkmk_purge)|新增|你现在可以通过清除早于指定日期的 IP 地址使用率数据来减少 IPAM 数据库的大小。|  
|[支持基于角色的访问控制的 Windows PowerShell](#bkmk_ps)|新增|可以使用 Windows PowerShell 设置 IPAM 对象上的访问作用域。|  
  
### <a name="EIP"></a>增强的 IP 地址管理  
以下功能改进了 IPAM 地址管理功能。  
>[!NOTE]
>有关 IPAM Windows PowerShell 命令参考，请参阅[Windows PowerShell 中的 IP 地址管理（IPAM）服务器 cmdlet](https://docs.microsoft.com/powershell/module/ipamserver/)。  
  
#### <a name="support-for-31-32-and-128-subnets"></a>支持/31、/32 和/128 子网  
Windows Server 2016 中的 IPAM 现在支持/31、/32 和/128 子网。 例如，对于交换机之间的点对点链路，可能需要两个地址子网（/31 个 IPv4）。 此外，某些交换机可能需要单个环回地址（对于 IPv4，为/32，对于 IPv6，则为128）。  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**查找具有 IpamFreeSubnet 的免费子网**  
  
此命令返回可用于分配的子网，给定 IP 块、前缀长度和请求的子网数。   
  
如果可用子网数小于请求的子网数，则将返回可用子网，并显示一条警告，指示可用的数目小于请求的数量。  
  
>[!NOTE]
>此函数不会实际分配子网，它只报告其可用性。 但是，可以通过管道将 cmdlet 输出传递给**IpamSubnet**命令，以创建子网。  
  
有关详细信息，请参阅[IpamFreeSubnet](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeSubnet)。  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**查找 IpamFreeRange 的可用地址范围**  
  
此新命令将返回给定 IP 子网的可用 IP 地址范围，该范围中所需的地址数，以及所请求的范围数。   
  
命令搜索与请求的地址数匹配的一系列未分配的 IP 地址。 将重复此过程，直到找到请求的范围数，或直到没有更多可用的地址范围。  
  
> [!NOTE]
> 此函数不会实际分配范围，只会报告其可用性。 但是，可以通过管道将 cmdlet 输出传递给**IpamRange**命令，以创建范围。  
  
有关详细信息，请参阅[IpamFreeRange](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeRange)。  
  
### <a name="EDNS"></a>增强的 DNS 服务管理  
Windows Server 2016 中的 IPAM 现在支持在 IPAM 运行的 Active Directory 林中发现基于文件的已加入域的 DNS 服务器。  
  
此外，还添加了以下 DNS 函数：  
  
-   来自运行 Windows Server 2008 或更高版本的 DNS 服务器的 DNS 区域和资源记录集合（与 DNSSEC 相关的）。  
  
-   针对所有类型的资源记录（而不是与 DNSSEC 相关的记录）配置（创建、修改和删除）属性和操作。  
  
-   配置（创建、修改、删除）所有类型的 DNS 区域（包括主辅助区域和存根区域）的属性和操作。  
  
-   辅助区域和存根区域上触发的任务，而不考虑它们是否是正向或反向查找区域。 例如，**从 Master 传输**的任务或**从 master 传输的新区域副本**等任务。  
  
-   支持的 DNS 配置的基于角色的访问控制（DNS 记录和 DNS 区域）。  
  
-   条件转发器集合和配置（创建、删除、编辑）。  
  
### <a name="DDI"></a>集成的 DNS、DHCP 和 IP 地址（DDI）管理  
当你在 IP 地址清单中查看 IP 地址时，你可以在详细信息视图中查看与该 IP 地址关联的所有 DNS 资源记录。  
  
作为 DNS 资源记录集合的一部分，IPAM 收集 DNS 反向查找区域的 PTR 记录。 对于映射到任何 IP 地址范围的所有反向查找区域，IPAM 将为相应映射的 IP 地址范围内属于该区域的所有 PTR 记录创建 IP 地址记录。 如果 IP 地址已经存在，则 PTR 记录只与该 IP 地址相关联。 如果反向查找区域未映射到任何 IP 地址范围，则不会自动创建 IP 地址。  
  
通过 IPAM 在反向查找区域中创建 PTR 记录时，将按照上述方式更新 IP 地址清单。 在后续收集期间，由于 IP 地址已存在于系统中，PTR 记录将只是与该 IP 地址映射。  
  
### <a name="bkmk_ad"></a>多 Active Directory 林支持  
在 Windows Server 2012 R2 中，IPAM 能够发现和管理与 IPAM 服务器属于同一个 Active Directory 林的 DNS 和 DHCP 服务器。 现在，你可以管理属于不同 AD 林的 DNS 和 DHCP 服务器与安装 IPAM 服务器的林之间存在双向信任关系。 可以在 "**配置服务器发现**" 对话框中，添加要管理的其他受信任林中的域。 发现服务器后，管理体验与安装 IPAM 的林中的服务器的管理体验相同。  
  
有关详细信息，请参阅[管理多个 Active Directory 林中的资源](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>清除利用率数据  
清除利用率数据允许您通过删除旧 IP 地址利用率数据来减少 IPAM 数据库的大小。 若要执行数据删除，请指定日期，IPAM 会删除早于或等于您提供的日期的所有数据库条目。   
  
有关详细信息，请参阅[清除利用率数据](../../technologies/ipam/Purge-Utilization-Data.md)。  
  
### <a name="bkmk_ps"></a>支持基于角色的访问控制的 Windows PowerShell  
你现在可以使用 Windows PowerShell 配置基于角色的访问控制。 你可以使用 Windows PowerShell 命令来检索 IPAM 中的 DNS 和 DHCP 对象并更改其访问作用域。 因此，你可以编写 Windows PowerShell 脚本，以将访问作用域分配给以下对象。  
  
-   IP 地址空间  
  
-   IP 地址块  
  
-   IP 地址子网  
  
-   IP 地址范围  
  
-   DNS 服务器  
  
-   DNS 区域  
  
-   DNS 条件转发器  
  
-   DNS 资源记录  
  
-   DHCP 服务器  
  
-   DHCP 超级作用域  
  
-   DHCP 作用域  
  
有关详细信息，请参阅在 Windows PowerShell 中使用 Windows PowerShell 和[IP 地址管理（IPAM）服务器 Cmdlet](https://docs.microsoft.com/powershell/module/ipamserver/)[管理基于角色的访问控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)。  

