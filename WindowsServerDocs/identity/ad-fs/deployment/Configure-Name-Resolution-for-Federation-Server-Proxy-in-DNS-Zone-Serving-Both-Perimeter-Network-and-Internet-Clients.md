---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: 在为外围网络和 Internet 客户端提供服务的 DNS 区域中为联合服务器代理配置名称解析
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 118c03ada32d3cd5b198ecd238078984a38df0db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359830"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>在为外围网络和 Internet 客户端提供服务的 DNS 区域中为联合服务器代理配置名称解析


因此，名称解析可以在 Active Directory 联合身份验证服务 \(AD FS @ no__t 方案中成功地运行联合服务器代理，其中一个或多个域名系统 \(DNS @ no__t 区域同时为外围网络和 Internet 提供服务客户端，必须完成下列任务：  
  
-   必须配置要控制的 Internet 区域中的 DNS，以将 AD FS 主机名的所有 Internet 客户端请求解析为联合服务器代理。 为此，请向联合服务器代理的 Internet DNS 区域添加一个主机 \(A @ no__t 资源记录。  
  
-   外围网络中的 DNS 必须配置为将 AD FS 主机名的所有传入客户端请求解析为联合服务器。 为此，请将一个主机 \(A @ no__t 资源记录添加到联合服务器代理的外围 DNS 区域。  
  
> [!NOTE]  
> 假设已在公司网络 DNS 中为联合服务器创建了主机 @no__t 0A @ no__t 资源记录。 如果此记录尚不存在，请创建此记录，然后执行这些过程。 有关如何为联合服务器创建主机 \(A @ no__t 资源记录的详细信息，请参阅[向联合服务器的企业 DNS 添加&#40;主机&#41; a 资源记录](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>向联合服务器代理的 Internet DNS 区域添加主机 @no__t 0A @ no__t 资源记录  
为了使 Internet 上的客户端计算机可以通过新部署的联合服务器代理成功访问联合服务器，你必须首先在你控制的 Internet DNS 区域中创建主机 @no__t 0A @ no__t 资源记录。 此资源记录将帐户联合服务器 @no__t 的主机名（0for） no__t-1 解析为帐户联合服务器 @no__t 代理的 IP 地址（在外围网络中 131.107.27.68 @ no__t-3）。  
  
> [!NOTE]  
> 假设你使用的 DNS 服务器运行 Windows 2000 Server、Windows Server 2003 或 Windows Server 2008 with DNS 服务器服务来控制 Internet DNS 区域。  
  
**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>向联合服务器代理的 Internet DNS 区域添加主机 \(A @ no__t 资源记录  
  
1.  在 DNS 服务器上的 Internet DNS 区域中，打开 DNS snap @ no__t-0in。  
  
2.  在控制台树中，右键 @ no__t-0click 适用的正向查找区域，然后单击 "**新建主机 \(a 或 AAAA @ no__t**"。  
  
3.  在 "**名称**" 中，仅键入联合服务器的计算机名称。 例如，对于完全限定的域名 \(FQDN @ no__t-1 fs.fabrikam.com，请键入**fs**。  
  
4.  在 " **IP 地址**" 中，键入新联合服务器代理的 IP 地址，例如131.107.27.68。  
  
5.  单击“添加主机”。  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>向联合服务器代理的外围 DNS 区域添加主机 @no__t 0A @ no__t 资源记录  
为了使 Internet 客户端请求可由联合服务器代理成功处理并在由 Internet DNS 区域解析后访问联合服务器，你必须在外围 DNS 区域中创建主机 @no__t 0A @ no__t 资源记录。 此资源记录解析帐户联合服务器 @no__t 的主机名，例如 fs 0for。 fabrikam .com @ no__t-0 到帐户联合服务器的 IP 地址 \(for 示例，企业网络中的 192.168.1.4 @ no__t。  
  
> [!NOTE]  
> 假设你使用的 DNS 服务器运行的是 Windows 2000 Server、Windows Server 2003、Windows Server 2008 或 Windows Server®2012，其中 DNS 服务器服务用于控制外围 DNS 区域。  
  
**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>向联合服务器代理的外围 DNS 区域添加主机 \(A @ no__t 资源记录  
  
1.  在外围网络的 DNS 服务器上，打开**dns snap @ no__t-1in**。  
  
2.  在控制台树中，右键 @ no__t-0click 适用的正向查找区域，然后单击 "**新建主机 \(a 或 AAAA @ no__t**"。  
  
3.  在 "**名称**" 中，仅键入联合服务器的计算机名称。 例如，对于 FQDN fs.fabrikam.com，键入 **fs**。  
  
4.  在 " **IP 地址**" 文本框中，键入企业网络中联合服务器的 IP 地址，例如192.168.1.4。  
  
5.  单击“添加主机”。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[联合服务器代理的名称解析要求](https://technet.microsoft.com/library/dd807055.aspx)  
  

