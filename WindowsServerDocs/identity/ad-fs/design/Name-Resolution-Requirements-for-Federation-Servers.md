---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: 联合服务器的名称解析要求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74701cbaa403611b081942f016b21db1c0b3ff70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845458"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>联合服务器的名称解析要求

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

当企业网络上的客户端计算机尝试访问应用程序或由 Active Directory 联合身份验证服务保护的 Web 服务\(AD FS\)，他们必须首先进行身份验证到联合身份验证服务器。 若要进行身份验证的一种方法是让企业网络客户端通过 Windows 集成身份验证访问本地联合身份验证服务器。  
  
## <a name="configure-corporate-dns"></a>配置企业 DNS  
以便进行本地联合身份验证服务器上通过 Windows 集成身份验证成功的名称解析，域名系统\(DNS\)企业网络中的帐户伙伴必须配置为新的主机\(A\)将解析的完全限定的域名的资源记录\(FQDN\)联合身份验证服务器与联合服务器群集的 IP 地址的主机名。  
  
在下图中，可以看到如何在给定方案中完成此任务。 在此方案中，Microsoft 网络负载平衡\(NLB\)为现有的联合服务器场提供单个群集的 FQDN 名称和单个群集 IP 地址。  
  
![名称要求](media/adfs2_deploy_single_fs.gif)  
  
有关如何配置群集 IP 地址或群集 FQDN 使用 NLB 的信息，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
有关如何配置公司 DNS 中的为联合身份验证服务器的信息，请参阅[添加主机&#40;A&#41;资源记录到公司 DNS 中为联合身份验证服务器](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
有关如何在外围网络中配置联合服务器代理的信息，请参阅[联合服务器代理的名称解析要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  

## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
