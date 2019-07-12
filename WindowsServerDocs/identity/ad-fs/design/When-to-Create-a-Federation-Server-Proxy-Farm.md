---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: 何时创建联合服务器代理场
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c33475d7420383448439e2b769562e55127c7b0e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190627"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>何时创建联合服务器代理场

请考虑安装其他联合服务器代理，如果有大型的 Active Directory 联合身份验证服务\(AD FS\)部署并且想要提供容错、 负载\-均衡和可伸缩性代理部署。 在相同外围网络中创建两个或多个联合服务器代理和配置每个保护相同的 AD FS 联合身份验证服务的操作将创建联合服务器代理场。  
  
您可以创建联合服务器代理场或使用 AD FS 联合服务器代理配置向导，向现有场安装其他联合服务器代理。 有关详细信息，请参阅[何时创建联合服务器代理](When-to-Create-a-Federation-Server-Proxy.md)。  
  
所有联合服务器代理可以一起都用作场之前，您必须首先群集，然后它们在一个 IP 地址和一个域名系统\(DNS\)完全限定的域名\(FQDN\)。 您可以通过部署 Microsoft 网络负载平衡群集服务器\(NLB\)外围网络中。 下表中的任务需要 NLB 能够正确配置为联合服务器代理场中的创建群集。  
  
有关如何为使用 Microsoft NLB 技术为群集配置 FQDN 的详细信息，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?linkid=74651)。  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>为场配置联合服务器代理  
下表介绍，以便每个联合服务器代理可以加入场中必须完成的任务。  
  
|任务|描述|  
|--------|---------------|  
|场中的所有代理都指向相同 AD FS 联合身份验证服务名称|在创建联合服务器代理时，你必须将参与场的所有联合服务器代理的 AD FS 联合服务器代理配置向导中键入相同的联合身份验证服务名称。 联合服务器代理使用组成此 DNS 主机名，以确定哪些 AD FS 联合身份验证服务实例其联系人的 URL。<br /><br />有关详细信息，请参阅[将计算机配置为联合服务器代理角色](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。|  
|获取和共享证书|您可以从获取服务器身份验证证书的公共证书颁发机构\(CA\)— 例如 VeriSign，然后配置该证书，以便所有联合服务器代理都共享的相同私钥部分的每个联合服务器代理默认网站上的相同证书。 若要共享证书，你必须安装相同的服务器身份验证证书在默认网站上为每个联合服务器代理。 有关详细信息，请参阅[服务器身份验证证书导入到默认网站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。<br /><br />有关详细信息，请参阅[联合服务器代理的证书要求](Certificate-Requirements-for-Federation-Server-Proxies.md)。|  
  
有关添加新的联合服务器代理创建联合服务器代理场的详细信息，请参阅[核对清单：设置联合服务器代理](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
