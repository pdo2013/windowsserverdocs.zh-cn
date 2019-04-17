---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: "何时创建联盟服务器代理电场的日落"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8935760cad272d5b82edb675cda85caf0456565f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>何时创建联盟服务器代理电场的日落

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

请考虑当较大的 Active Directory 联合身份验证服务 \(AD FS\) 部署并且你想要提供代理部署故障、load\ 平衡和可扩展性安装其他联合身份验证的服务器代理。 在同一外围网络创建两个或多个联合身份验证的代理服务器和配置每个保护相同广告 FS 联合身份验证服务法案创建联盟服务器代理场。  
  
你可以创建联合身份验证的服务器代理场或使用广告 FS 联合身份验证的代理配置向导到现有场安装其他联合身份验证的服务器代理。 有关详细信息，请参阅[何时创建联合身份验证的服务器代理](When-to-Create-a-Federation-Server-Proxy.md)。  
  
所有联合 server 代理可以一起为场起都作用之前，你必须先群集它们下一个 IP 地址和一个域名系统 \(DNS\) 完整的域命名 \(FQDN\)。 你可以通过部署周边网络内的 Microsoft 网络负载平衡 \(NLB\) 群集服务器。 下表中的任务需要 NLB 相应地进行配置为群集场中的联合 server 代理。  
  
有关如何配置为使用 Microsoft NLB 技术的群集 FQDN 的详细信息，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?linkid=74651)。  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>配置联合身份验证的服务器代理电场的日落  
下表介绍了，以便每个联合身份验证的服务器代理可以参与电场的日落，必须先完成的任务。  
  
|任务|描述|  
|--------|---------------|  
|指向广告 FS 联合身份验证服务同名的一场中的所有代理服务器|当您创建联合身份验证的代理服务器时，你必须将参与农场里的所有联合服务器代理的广告 FS 联合身份验证的代理配置向导中键入相同的联合身份验证服务名称。 联合身份验证的服务器代理使用占据以确定哪个广告 FS 联合身份验证服务实例此 DNS 主机名该联系人的 URL。<br /><br />有关详细信息，请参阅[将计算机配置为联合身份验证的服务器代理角色](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。|  
|获取和共享证书|你可以获取服务器身份验证证书公共证书颁发机构从 \ (CA\)-例如，VeriSign，然后配置证书，以便所有联合身份验证的服务器代理服务器每个联合身份验证的服务器代理分享了相同专用同一个证书默认网站上的主要部分。 若要共享的证书，必须在每个联合身份验证的服务器代理默认网站上安装相同服务器身份验证证书。 有关详细信息，请参阅[将服务器身份验证证书导入默认网站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。<br /><br />有关详细信息，请参阅[证书要求联合身份验证的服务器代理](Certificate-Requirements-for-Federation-Server-Proxies.md)。|  
  
有关添加新的联合 server 代理创建联盟服务器代理农场里的详细信息，请参阅[清单：设置向上联盟服务器代理](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
