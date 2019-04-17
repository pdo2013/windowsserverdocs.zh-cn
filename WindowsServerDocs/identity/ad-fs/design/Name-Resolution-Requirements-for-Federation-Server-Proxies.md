---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: "联合身份验证的服务器代理名称分辨率要求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a94e4de181cd8794d479bbd6695a94658aba0f86
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>联合身份验证的服务器代理名称分辨率要求

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

当在 Internet 上的客户端尝试访问的应用程序受 Active Directory 联合身份验证服务 \(AD FS\) 时，他们必须首次进行身份验证联合身份验证的服务器。 在大多数情况下，联盟服务器不可通常直接通过 Internet 访问。 因此的客户端计算机 Internet 必须重定向到联合身份验证的服务器代理改为。 你可以通过向你 DNS 区域或面临 Internet 的区域中添加相应域名系统 \(DNS\) 记录完成成功重定向。  
  
将 Internet 客户端重定向到联合身份验证的服务器代理你使用的方法取决于中外围网络配置 DNS 区域的方式或 Internet 上配置您控制 DNS 区域的方式。 联合身份验证的服务器代理供外围网络中的使用。 它们 Internet 客户端将请求重定向到联合身份验证的服务器成功仅当 DNS 具有正确配置控制你的所有 Internet \-facing 区域中。 因此，你的 Internet \-facing 区域配置，你是否具有 DNS 区域投放仅外围网络或服务外围网络和 Internet 客户端 DNS 区域，非常重要。  
  
本主题介绍了时在外围网络位置联合身份验证的服务器代理配置名称分辨率可以采取的步骤。 若要确定要遵循的步骤，首先确定哪些以下 DNS 情景最匹配的你的组织外围网络中的 DNS 基础结构。 然后按照该方案的步骤。  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>投放仅外围网络 DNS 区域  
在此情况下，您的组织有一个或两个 DNS 区域外围网络中的，并在 Internet 上的任何 DNS 区域不控制你的组织。 成功名称 DNS 区域所服务周边的网络方案中联盟服务器代理服务器的分辨率取决于将根据以下条件：  
  
-   联合身份验证的服务器代理必须安装一个设置来解决完全限定的域主机文件中命名 \(FQDN\) 联合身份验证的服务器端点 URL 到联合服务器或联合身份验证的服务器群集的 IP 地址。  
  
-   必须配置的帐户合作伙伴外围网络中的 DNS，以便联合身份验证的服务器端点 URL FQDN 解析为联合身份验证的服务器代理的 IP 地址。  
  
下图和相应步骤显示给定的示例如何实现每种情况。 在此示例中，Microsoft 网络负载平衡 \(NLB\) 技术的现有联盟服务器场提供一个、群集 FQDN 和单、群集运行 IP 地址。  
  
![名称要求](media/adfs2_deploy_single_fs.gif)  
  
有关配置群集 IP 地址或群集 FQDN 使用 NLB，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1.配置主机上联合身份验证的服务器代理文件  
帐户合作伙伴联合身份验证的服务器代理外围网络中的 DNS 配置为解决帐户联合身份验证的服务器代理所有请求 fs.fabrikam.com，因为在其本地主机文件来都解决的实际帐户联合身份验证的服务器的 IP 地址 fs.fabrikam.com 具有条目 \（或联合身份验证的服务器 farm\ 群集 DNS 名称），连接到公司的网络。 这使得帐户联合身份验证的服务器代理解决主机名称 fs.fabrikam.com 帐户联合身份验证的服务器，而不是本身，会发生时，如果它已尝试查找 fs.fabrikam.com 使用外围 DNS，以便联合身份验证的服务器代理可以通信联合身份验证的服务器。  
  
### <a name="2-configure-perimeter-dns"></a>2.配置外围 DNS  
因为仅单个广告 FS 主机名称客户端计算机将定向到，它们是否或 Internet 上的 intranet-使用外围 DNS 服务器的客户端计算机上的 Internet 必须解决有关帐户联合身份验证的服务器 \(fs.fabrikam.com\) FQDN 帐户联合 server 代理外围网络上的 IP 地址。 以便在尝试解决 fs.fabrikam.com 时，它可以转发保持帐户联合身份验证的服务器代理客户端，周边 DNS 包含有限的 corp.fabrikam.com DNS 区域 fs \(fs.fabrikam.com\) 单个主机 \(A\) 资源记录与帐户联合 server 代理外围网络上的 IP 地址。  
  
有关如何修改联合身份验证的服务器代理服务器主机文件和配置 DNS 外围网络中的详细信息，请参阅[配置的联合 Server 代理提供仅外围网络 DNS 区域中的名称分辨率](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)。  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>DNS 区域服务外围网络和 Internet 客户端  
在此情况下，你的组织控制 DNS 区域中外围网络和 Internet 上的至少一个 DNS 区域。 在此情况下联盟服务器代理服务器的成功名称分辨率取决于将根据以下条件：  
  
-   必须配置的帐户合作伙伴 Internet 区域中的 DNS，以便联合身份验证的服务器主机名 FQDN 解析为联合 server 代理外围网络中的 IP 地址。  
  
-   必须配置的帐户合作伙伴外围网络中的 DNS，以便联合身份验证的服务器主机名 FQDN 解决到公司的网络中的联合服务器的 IP 地址。  
  
下图和相应步骤显示给定的示例如何实现每种情况。  
  
![名称要求](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1.配置外围 DNS  
这种情况下，这是因为假定您将配置 Internet DNS 区域该您可以控制要解决请求进行特定端点 url \ (即 fs.fabrikam.com\) 到联合 server 代理外围网络中，您还必须配置区域中外围 DNS 转发到公司的网络中的联合服务器这些请求。  
  
以便客户可以转发到帐户联盟服务器，在尝试解决 fs.fabrikam.com 时，周边 DNS 配置为使用一个主机 \(A\) 资源记录 fs \(fs.fabrikam.com\) 和帐户联盟服务器公司的网络上的 IP 地址。 这使得帐户联合身份验证的服务器代理解决主机名称 fs.fabrikam.com 帐户联合身份验证的服务器，而不是本身，会发生时，如果需要使用 Internet DNS fs.fabrikam.com 查找尝试执行，以便联合身份验证的服务器代理可以通信联合身份验证的服务器。  
  
### <a name="2-configure-internet-dns"></a>2.配置 Internet DNS  
名称分辨率成功在此情况下，从 Internet 上的客户端计算机的所有请求到 fs.fabrikam.com 必须都解决通过 Internet DNS 区域你控制。 因此，你必须配置转发 fs.fabrikam.com 帐户联盟服务器代理服务器外围网络中的 IP 地址的客户端请求你 Internet DNS 区域。  
  
有关如何修改外围网络和 Internet DNS 区域的详细信息，请参阅[联盟服务器代理服务器 DNS 区域，有两个外围网络和 Internet 客户端中的配置名称分辨率](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
