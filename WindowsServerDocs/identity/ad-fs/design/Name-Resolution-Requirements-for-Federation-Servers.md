---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: "联合身份验证的服务器的名称分辨率要求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74701cbaa403611b081942f016b21db1c0b3ff70
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-servers"></a>联合身份验证的服务器的名称分辨率要求

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

当客户端计算机上尝试访问的应用程序或 Web 服务中受 Active Directory 联合身份验证服务 \(AD FS\) 企业网络，他们必须首次进行身份验证联合服务器。 验证身份一个方法是能够访问本地联盟服务器通过 Windows 的集成身份验证的企业网络客户端。  
  
## <a name="configure-corporate-dns"></a>配置公司 DNS  
以便进行成功名称的分辨率，通过 Windows 的集成身份验证本地联合身份验证的服务器，必须将解决联合身份验证的服务器群集的 IP 地址的完整的名称 \(FQDN\) 主机域名联合身份验证的服务器新主机 \(A\) 资源记录配置域名系统 \(DNS\) 中的帐户的合作伙伴公司的网络。  
  
在下图中，您可以看到给定的方案来完成此任务中的如何。 在此情况下，Microsoft 网络负载平衡 \(NLB\) 的现有联盟服务器场提供一个群集 FQDN 名称和单个群集 IP 地址。  
  
![名称要求](media/adfs2_deploy_single_fs.gif)  
  
有关如何群集 FQDN 使用 NLB 或配置群集 IP 地址的信息，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
有关如何配置公司 DNS 服务器是否联合身份验证信息，请参阅[添加主机 & #40;一个 & #41;资源录制到公司 DNS 服务器是否联盟](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
有关如何将服务器联合身份验证的代理配置外围网络中的信息，请参阅[联合身份验证的服务器代理名称分辨率要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  

## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
