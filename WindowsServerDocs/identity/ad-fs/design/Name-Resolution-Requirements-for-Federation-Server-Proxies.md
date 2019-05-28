---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: 联合服务器代理的名称解析要求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8aef8b3d8f1e6dde4f960a3bee5a93964d07c72b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191285"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>联合服务器代理的名称解析要求

当在 Internet 上的客户端计算机尝试访问受 Active Directory 联合身份验证服务的应用程序\(AD FS\)，他们必须首先进行身份验证到联合身份验证服务器。 在大多数情况下，联合身份验证服务器通常是不能从 Internet 直接访问。 因此，Internet 客户端计算机必须重定向到联合服务器代理改为。 您可以成功完成重定向通过添加相应域名系统\(DNS\)到你的 DNS 区域或多个面向 Internet 的区域的记录。  
  
使用 Internet 客户端重定向到联合服务器代理的方法取决于如何在外围网络中配置的 DNS 区域或如何在 Internet 上配置可以控制 DNS 区域。 联合服务器代理专门用于外围网络。 它们 Internet 客户端将请求重定向到联合身份验证服务器成功所有 Internet 中正确配置 DNS 时仅\-面向您控制的区域。 因此，Internet 配置\-面向区域，是否可以仅为外围网络的 DNS 区域或外围网络和 Internet 客户端提供服务的 DNS 区域-非常重要。  
  
本主题介绍可用于配置名称解析，当你将联合服务器代理放在外围网络中的步骤。 若要确定所要遵循的步骤，请先确定以下 DNS 方案中的哪一个与组织外围网络中的 DNS 基础结构最匹配。 然后，遵循该方案中的步骤。  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>仅为外围网络提供服务的 DNS 区域  
在此方案中，组织在外围网络中有一个或两个 DNS 区域，并且组织在 Internet 上未控制任何 DNS 区域。 仅为外围网络方案提供服务的 DNS 区域中的联合身份验证服务器代理的成功的名称解析取决于以下条件：  
  
-   联合服务器代理必须具有一个设置要解析的完全限定的域名的主机文件中\(FQDN\)的联合身份验证服务器终结点 URL 为联合身份验证服务器或联合服务器群集的 IP 地址。  
  
-   必须配置帐户伙伴的外围网络中的 DNS，以便联合身份验证服务器终结点 URL 的 FQDN 解析为联合服务器代理的 IP 地址。  
  
下面的插图和相应步骤显示了如何在给定示例中实现以上每种条件。 在此图中，Microsoft 网络负载平衡\(NLB\)技术提供了一个群集的 FQDN 和单个现有的联合服务器场的群集 IP 地址。  
  
![名称要求](media/adfs2_deploy_single_fs.gif)  
  
有关配置群集 IP 地址或群集 FQDN 的详细信息使用 NLB，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1.配置联合服务器代理上的主机文件  
帐户伙伴联合服务器代理在外围网络中的 DNS 配置为将针对 fs.fabrikam.com 的所有请求解析到帐户联合服务器代理，因为其本地主机文件以将 fs.fabrikam.com 解析为的 IP 地址中有条目实际帐户联合身份验证服务器\(联合服务器场的群集 DNS 名称或\)为连接到公司网络。 这使得帐户联合服务器代理，以解析主机名 fs.fabrikam.com 解析到帐户联合身份验证服务器而不是本身 — 如果它尝试查找 fs.fabrikam.com 使用外围 DNS，以便联合身份验证服务器代理与联合身份验证服务器可以通信。  
  
### <a name="2-configure-perimeter-dns"></a>2.配置外围 DNS  
因为只有单个 AD FS 主机名称，客户端计算机定向到 — 无论是在 intranet 上还是在 Internet 上，在 Internet 上使用外围 DNS 服务器的客户端计算机必须将该 FQDN 解析为帐户联合身份验证服务器\(fs.fabrikam.com\)到外围网络上帐户联合服务器代理的 IP 地址。 在外围网络 DNS，以便在尝试解析 fs.fabrikam.com 时，它可以将转发到帐户联合服务器代理的客户端，包含具有单个主机的受限的 corp.fabrikam.com DNS 区域\(A\)资源记录，用于 fs \(fs.fabrikam.com\)和外围网络上帐户联合服务器代理的 IP 地址。  
  
有关如何修改主机文件中的联合服务器代理和外围网络中配置 DNS 的详细信息，请参阅[DNS 区域提供服务仅为外围网络中的联合服务器代理的配置名称解析](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>同时为外围网络和 Internet 客户端提供服务的 DNS 区域  
在此方案中，组织控制外围网络中的 DNS 区域以及 Internet 上的至少一个 DNS 区域。 在此方案中的联合身份验证服务器代理的成功的名称解析取决于以下条件：  
  
-   必须配置帐户伙伴的 Internet 区域中的 DNS，以便联合身份验证服务器主机名的 FQDN 解析为外围网络中的联合身份验证服务器代理的 IP 地址。  
  
-   必须配置帐户伙伴的外围网络中的 DNS，以便联合身份验证服务器主机名的 FQDN 解析为公司网络中的联合身份验证服务器的 IP 地址。  
  
下面的插图和相应步骤显示了如何在给定示例中实现以上每种条件。  
  
![名称要求](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1.配置外围 DNS  
对于此方案，因为它假定你将配置 Internet DNS 区域所控制要解析的请求特定的终结点 url\(即，fs.fabrikam.com\)到中的联合身份验证服务器代理外围网络中，您还必须配置该区域在外围网络 DNS 将这些请求转发到公司网络中的联合身份验证服务器中。  
  
一台主机，以便在尝试解析 fs.fabrikam.com 时，客户端可以转发到帐户联合身份验证服务器，配置外围 DNS \(A\)资源记录，用于 fs \(fs.fabrikam.com\)和公司网络上的帐户联合身份验证服务器的 IP 地址。 这使得帐户联合服务器代理，以解析主机名 fs.fabrikam.com 解析到帐户联合身份验证服务器而不是本身 — 如果它尝试查找 fs.fabrikam.com 使用 Internet DNS，以便联合身份验证服务器代理可以与联合身份验证服务器进行通信。  
  
### <a name="2-configure-internet-dns"></a>2.配置 Internet DNS  
若要在此方案中成功进行名称解析，必须由所控制的 Internet DNS 区域解析 Internet 上的客户端计算机针对 fs.fabrikam.com 发出的所有请求。 因此，必须配置 Internet DNS 区域将 fs.fabrikam.com 解析到帐户联合身份验证服务器代理在外围网络中的 IP 地址的客户端请求转发。  
  
有关如何修改外围网络和 Internet DNS 区域的详细信息，请参阅[联合服务器代理在 DNS 区域，提供两个外围网络和 Internet 客户端的配置名称解析](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
