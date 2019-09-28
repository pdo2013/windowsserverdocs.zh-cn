---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: 联合服务器的名称解析要求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 88ec418bd72a6389856deb1abd85641d8782bc30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408046"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>联合服务器的名称解析要求

当企业网络上的客户端计算机尝试访问 Active Directory 联合身份验证服务 @no__t 保护的应用程序或 Web 服务时，必须先向联合服务器进行身份验证。 进行身份验证的一种方法是让企业网络客户端通过 Windows 集成身份验证访问本地联合服务器。  
  
## <a name="configure-corporate-dns"></a>配置企业 DNS  
若要在本地联合服务器上通过 Windows 集成身份验证成功进行名称解析，则必须为新的主机配置帐户伙伴的企业网络中的域名系统 \(DNS @ no__t-1 \(A @ no__t一种资源记录，它将联合服务器的完全限定域名 \(FQDN @ no__t-5 主机名解析为联合服务器群集的 IP 地址。  
  
在下图中，可以看到如何在给定方案中完成此任务。 在此方案中，Microsoft 网络负载平衡 \(NLB @ no__t）为现有联合服务器场提供单个群集的 FQDN 名称和单个群集 IP 地址。  
  
![名称要求](media/adfs2_deploy_single_fs.gif)  
  
有关如何使用 NLB 配置群集 IP 地址或群集 FQDN 的信息，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
有关如何为联合服务器配置企业 DNS 的信息，请参阅[向联合服务器的&#40;企业&#41; Dns 添加主机 a 资源记录](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
有关如何在外围网络中配置联合服务器代理的信息，请参阅[联合服务器代理的名称解析要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  

## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
