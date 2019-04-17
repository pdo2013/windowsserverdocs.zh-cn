---
title: 什么是 IPAM 中的新增功能
description: 本主题介绍了新的或更改在 Windows Server 2016 的 IP 地址管理 (IPAM) 功能。
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
ms.openlocfilehash: b90cd1ab223e38cbf5933b58a594b32d5e3d4858
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ipam"></a>什么是 IPAM 中的新增功能

>适用于：Windows Server（半年通道），Windows Server 2016

本主题介绍了新的或更改在 Windows Server 2016 的 IP 地址管理 (IPAM) 功能。  
  
IPAM 企业或云服务提供商 (CSP) 的网络上提供高度自定义管理和监控的 IP 地址和 DNS 基础结构的功能。 你可以监视、 审核，并管理动态主机配置协议 (DHCP) 和域名系统 (DNS) 运行，方法是使用 IPAM 服务器。  
  
## <a name="BKMK_IPAM2012R2"></a>在服务器 IPAM 的更新  
以下是在 Windows Server 2016 IPAM 新的和改进功能。  
  
|功能中的功能|新的或改进|描述|  
|--------------------------|-------------------|---------------|  
|[增强的 IP 地址管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|改进了|如处理 IPv4/32 和 IPv6 /128 子网和 IP 地址块中查找 IP 地址的免费网和范围方案 IPAM 功能得到改进。|  
|[增强的 DNS 点点](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|新增功能|IPAM 支持两个已加入域的 Active Directory 集成和文件备份 DNS 服务器的资源 DNS 记录、 条件转发器，并 DNS 区域管理。|  
|[集成 DNS、 DHCP 和 IP 地址 (DDI) 管理](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|改进了|启用操作，如直观显示所有 DNS 资源记录属于 IP 地址，几个新的体验和集成生命周期管理自动 IP 地址基于 DNS 资源的记录，以及对 DNS 和 DHCP 操作的 IP 地址生命周期管理的清单。|  
|[多个主动目录森林支持](#bkmk_ad)|新增功能|IPAM 可用于装有 IPAM 森林和每个远程林双向信任关系时管理多个 Active Directory 林 DHCP 和 DNS 服务器。|  
|[清除利用率数据](#bkmk_purge)|新增功能|你现在可以通过清除早于你所指定的日期的 IP 地址利用率数据降低 IPAM 数据库大小。|  
|[Windows PowerShell 角色基于访问控制支持](#bkmk_ps)|新增功能|你可以使用 Windows PowerShell 设置 IPAM 对象访问范围。|  
  
### <a name="EIP"></a>增强的 IP 地址管理  
以下功能改进 IPAM 地址管理功能。  
>[!NOTE]
>请 IPAM Windows PowerShell 命令参考[IP 地址管理 (IPAM) 服务器 Cmdlet 的 Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx)。  
  
#### <a name="support-for-31-32-and-128-subnets"></a>支持 /31、 / 32，以及 /128 子网  
在 Windows Server 2016 现在支持 /31、 / 32，以及 /128 子网 IPAM。 例如，两个地址子网 (月 31 IPv4) 可能需要点到点链接之间切换。 此外，某些开关可能需要一个回送地址 (/ 32 ipv4，/128 ipv6)。  
  
#### **<a name="find-free-subnets-with-find-ipamfreesubnet"></a>查找 IpamFreeSubnet 与的免费网**  
  
此命令返回子网所提供的分配，给定 IP 阻止、 前缀长度和大量请求个子网。   
  
如果可用子网小于请求子网的数量，可用子网返回的警告，指出提供的号码小于请求编号。  
  
>[!NOTE]
>此函数不会实际上分配子网，它仅报告这些应用。 但是，cmdlet 输出可以传送到**添加 IpamSubnet**创建网的命令。  
  
有关详细信息，请参阅[查找 IpamFreeSubnet](https://technet.microsoft.com/library/mt712782.aspx)。  
  
#### **<a name="find-free-address-ranges-with-find-ipamfreerange"></a>发现免费的地址与查找 IpamFreeRange 的范围**  
  
此新命令退货可用 ip 地址给定 IP 子网、 所需区域中的地址数和区域请求的数量。   
  
搜索未分配的 IP 地址相匹配的请求地址数连续系列该命令。 重复该过程，直到找到范围请求的数，则或地址的直到不再可用范围可用。  
  
> [!NOTE]
> 此函数实际上未分配的区域，则只会报告这些应用。 但是，cmdlet 输出可以传送到**添加 IpamRange**创建范围的命令。  
  
有关详细信息，请参阅[查找 IpamFreeRange](https://technet.microsoft.com/library/mt712772.aspx)。  
  
### <a name="EDNS"></a>增强的 DNS 点点  
在 Windows Server 2016 IPAM 现在支持 IPAM 正在运行 Active Directory 森林中的发现基于文件、 已加入域的 DNS 服务器。  
  
此外，已添加以下 DNS 功能：  
  
-   区域和资源会记录从运行 Windows Server 2008 或更高版本的 DNS 服务器 （而不是那些属于 DNSSEC） 的集锦。  
  
-   配置 （创建、 修改和删除） 属性并打开所有类型的资源记录 （而不是那些属于 DNSSEC） 的操作。  
  
-   配置 （创建、 修改、 删除） 属性并打开所有类型的 DNS 区域，包括主辅助和桩模块区域的操作)。  
  
-   如果将它们转发或反向查找区域来访触发辅助上的任务和桩模块区域。 例如，如任务**从主服务器传输**或**传输从主区域的新副本**。  
  
-   基于角色访问控制 （DNS 记录和 DNS 区域） 的支持 DNS 配置。  
  
-   条件转发器收集和配置创建、 删除 (编辑）。  
  
### <a name="DDI"></a>集成 DNS、 DHCP 和 IP 地址 (DDI) 管理  
当你查看的 IP 地址库存的 IP 地址时，你将可以选择在详细信息视图中查看所有 DNS 资源记录与 IP 地址相关联。  
  
作为一部分 DNS 资源录制集锦 IPAM 收集 PTR 记录 DNS 反向查找区域。 对于所有反向查找区域映射到任何 IP 地址范围，IPAM 创建属于该区域相应映射 IP 地址范围内的所有 PTR 记录 IP 地址记录。 如果已经存在的 IP 地址，PTR 记录是只需该 IP 地址相关联。 不会自动创建的 IP 地址，如果反向查找区域没有映射到任何 IP 地址范围。  
  
通过 IPAM 了反向查找区域中创建 PTR 记录时，如上所述方式相同更新 IP 地址库存。 在后续集锦，由于系统中已经存在的 IP 地址，将 PTR 录制将只需映射与这 IP 地址。  
  
### <a name="bkmk_ad"></a>多个主动目录森林支持  
在 Windows Server 2012 R2 IPAM 是能够发现和管理属于 IPAM 服务器同一个 Active Directory 林中 DHCP 和 DNS 服务器。 现在，你可以管理 DNS 服务器和 DHCP 服务器属于不同的广告森林时，它有与装有 IPAM 服务器森林双向信任关系。 你可以转到**配置服务器发现**对话框框中，并添加你想要管理的森林受信任从其他。 发现服务器管理体验后，与属于装有 IPAM 相同林服务器相同。  
  
有关详细信息，请参阅[管理资源多主动目录森林](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>清除利用率数据  
清除利用率数据可以通过删除旧的 IP 地址利用率数据减小 IPAM 数据库大小。 执行删除数据，你可以指定日期，然后 IPAM 删除所有数据库条目早于或提供等于日期。   
  
有关详细信息，请参阅[清除利用率数据](../../technologies/ipam/Purge-Utilization-Data.md)。  
  
### <a name="bkmk_ps"></a>Windows PowerShell 角色基于访问控制支持  
你现在可以使用 Windows PowerShell 配置角色基于访问控制。 可以使用 Windows PowerShell 命令检索 IPAM 中的 DNS 和 DHCP 对象并更改其访问权限的范围。 出于此原因，你还可以编写访问范围赋予以下对象 Windows PowerShell 脚本。  
  
-   IP 地址空间  
  
-   IP 地址阻止  
  
-   IP 地址子网  
  
-   Ip 地址  
  
-   DNS 服务器  
  
-   DNS 区域  
  
-   DNS 条件转发器  
  
-   DNS 资源记录  
  
-   DHCP 服务器  
  
-   DHCP 超级  
  
-   DHCP 范围  
  
有关详细信息，请参阅[管理角色基于访问控制与 Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)和[IP 地址管理 (IPAM) 服务器 Cmdlet 的 Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx)。  

