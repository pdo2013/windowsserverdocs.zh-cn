---
title: IPAM 中的新增功能
description: 本主题介绍了新增或更改 Windows Server 2016 中的 IP 地址管理 (IPAM) 功能。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1305f889339fb4ca6815912924ba2232cfaf4cab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880748"
---
# <a name="whats-new-in-ipam"></a>IPAM 中的新增功能

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍了新增或更改 Windows Server 2016 中的 IP 地址管理 (IPAM) 功能。  
  
IPAM 提供了企业版或云服务提供商 (CSP) 网络上的 IP 地址和 DNS 基础结构的高度可自定义管理和监视能力。 可以监视、 审核和管理使用 IPAM 运行动态主机配置协议 (DHCP) 和域名系统 (DNS) 服务器。  
  
## <a name="BKMK_IPAM2012R2"></a>中 IPAM 服务器的更新  
以下是 Windows Server 2016 中的 IPAM 的新增和改进功能。  
  
|特性/功能|新功能或改进功能|描述|  
|--------------------------|-------------------|---------------|  
|[增强的 IP 地址管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|改进功能|IPAM 功能被改进的方案，例如处理/32 IPv4 和 IPv6 为/128 子网和 IP 地址块中查找可用的 IP 地址子网和范围。|  
|[增强的 DNS 服务管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|新增|IPAM 支持 DNS 资源记录、 条件转发器和 DNS 区域管理这两个已加入域的 Active Directory 集成和支持文件的 DNS 服务器。|  
|[集成的 DNS、 DHCP 和 IP 地址 (DDI) 管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|改进功能|多个新体验和已启用集成的生命周期管理操作，例如可视化所有 DNS 资源记录适用于 IP 地址，基于 DNS 资源记录和 IP 地址生命周期管理的 IP 地址的自动化的清单有关 DNS 和 DHCP 的操作。|  
|[多个 Active Directory 林支持](#bkmk_ad)|新增|IPAM 可用于管理多个 Active Directory 林的 DNS 和 DHCP 服务器时安装 IPAM 的林和每个远程林之间具有双向信任关系。|  
|[清除利用率数据](#bkmk_purge)|新增|你现在可以通过清除早于指定日期的 IP 地址利用率数据减少 IPAM 数据库大小。|  
|[基于角色的访问控制的 Windows PowerShell 支持](#bkmk_ps)|新增|可以使用 Windows PowerShell IPAM 对象上设置访问作用域。|  
  
### <a name="EIP"></a>增强的 IP 地址管理  
以下功能来提高 IPAM 地址管理功能。  
>[!NOTE]
>有关 IPAM 的 Windows PowerShell 命令参考，请参阅[Windows PowerShell 中的 IP 地址管理 (IPAM) 服务器 Cmdlet](https://technet.microsoft.com/library/jj553807.aspx)。  
  
#### <a name="support-for-31-32-and-128-subnets"></a>支持/31、 / 32，以及为/128 子网  
Windows Server 2016 现在支持/31、 / 32 和为/128 子网中的 IPAM。 例如，两个地址子网 (/ 31 IPv4) 可能要求的交换机之间的点对点链路。 此外，某些开关可能需要单个环回地址 (/ 32 ipv4，对 IPv6 为/128)。  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**查找免费的子网，具有查找 IpamFreeSubnet**  
  
此命令返回的子网，可用于分配，给定一个 ip 地址块、 前缀长度和所请求的子网数。   
  
如果可用子网数小于所请求的子网数，可用子网将返回一个警告，指示可用的编号小于请求的数目。  
  
>[!NOTE]
>此函数不会实际分配子网，它只报告其可用性。 但是，cmdlet 输出可以输送到**添加 IpamSubnet**命令以创建子网。  
  
有关详细信息，请参阅[查找 IpamFreeSubnet](https://technet.microsoft.com/library/mt712782.aspx)。  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**查找可用的地址范围和查找 IpamFreeRange**  
  
此新命令返回可用 IP 地址范围在给定的 IP 子网、 在范围内，所需的地址数和所请求的范围数。   
  
此命令搜索的一系列连续的未分配的 IP 地址相匹配的请求的地址数。 重复该过程，直到找到请求的范围数，或直到再没有更多可用地址范围可用。  
  
> [!NOTE]
> 此函数不会实际分配的范围，它只报告其可用性。 但是，cmdlet 输出可以输送到**添加 IpamRange**命令以创建该范围。  
  
有关详细信息，请参阅[查找 IpamFreeRange](https://technet.microsoft.com/library/mt712772.aspx)。  
  
### <a name="EDNS"></a>增强的 DNS 服务管理  
Windows Server 2016 中的 IPAM 现在支持在其中运行 IPAM 的 Active Directory 林发现的基于文件的、 已加入域的 DNS 服务器。  
  
此外，已添加了以下 DNS 函数：  
  
-   DNS 区域和资源记录 （而不是 dnssec 相关的那些） 的集合从运行 Windows Server 2008 或更高版本的 DNS 服务器。  
  
-   配置 （创建、 修改和删除） 的属性和对所有类型 （而不是 dnssec 相关的那些） 资源记录的操作。  
  
-   配置 （创建、 修改、 删除） 的属性和操作对所有类型的 DNS 区域包括主辅助和存根区域)。  
  
-   如果正向或反向查找区域而不考虑触发辅助数据库上的任务和存根区域。 例如，如任务**从主区域传输**或**从主区域传输的区域的新副本**。  
  
-   受支持的 DNS 配置 （DNS 记录和 DNS 区域） 的基于角色的访问控制。  
  
-   条件转发器集合和配置 （创建、 删除、 编辑）。  
  
### <a name="DDI"></a>集成的 DNS、 DHCP 和 IP 地址 (DDI) 管理  
当 IP 地址清单中查看 IP 地址时，您可以在详细信息视图以查看与 IP 地址相关联的所有 DNS 资源记录。  
  
DNS 资源记录集合的一部分，IPAM 会收集 DNS 反向查找区域的 PTR 记录。 对于所有反向查找区域映射到的任何 IP 地址范围，IPAM 将创建属于该区域的相应映射的 IP 地址范围中的 PTR 记录的 IP 地址记录。 如果已存在该 IP 地址，是只需与该 IP 地址相关联的 PTR 记录。 如果反向查找区域未映射到的任何 IP 地址范围的 IP 地址不会自动创建。  
  
IPAM 通过反向查找区域中创建 PTR 记录后，按上文所述相同的方式更新 IP 地址清单。 在后续回收期间由于在系统中，已存在的 IP 地址的 PTR 记录将只需映射与该 IP 地址。  
  
### <a name="bkmk_ad"></a>多个 Active Directory 林支持  
在 Windows Server 2012 R2 中，IPAM 就能够发现和管理属于与 IPAM 服务器相同的 Active Directory 林的 DNS 和 DHCP 服务器。 现在你可以管理 DNS 和 DHCP 服务器属于不同的 AD 林具有双向信任关系的林中安装 IPAM 服务器时。 你可以转到**配置服务器发现**对话框框中，添加来自其他受你想要管理的林。 发现服务器后，管理体验是属于同一林安装 IPAM 的服务器相同。  
  
有关详细信息，请参阅[管理多个 Active Directory 林中的资源](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>清除利用率数据  
清除利用率数据，可通过删除旧的 IP 地址利用率数据来减小 IPAM 数据库的大小。 若要执行删除数据后，你指定的日期，和 IPAM 中删除早于的所有数据库条目或提供的日期。   
  
有关详细信息，请参阅[清除利用率数据](../../technologies/ipam/Purge-Utilization-Data.md)。  
  
### <a name="bkmk_ps"></a>基于角色的访问控制的 Windows PowerShell 支持  
现在可以使用 Windows PowerShell 来配置基于角色的访问控制。 可以使用 Windows PowerShell 命令来检索在 IPAM 中的 DNS 和 DHCP 对象并更改其访问作用域。 因此，可以编写 Windows PowerShell 脚本来将访问作用域分配给以下对象。  
  
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
  
有关详细信息，请参阅[管理基于角色的访问控制使用 Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)并[Windows PowerShell 中的 IP 地址管理 (IPAM) 服务器 Cmdlet](https://technet.microsoft.com/library/jj553807.aspx)。  

