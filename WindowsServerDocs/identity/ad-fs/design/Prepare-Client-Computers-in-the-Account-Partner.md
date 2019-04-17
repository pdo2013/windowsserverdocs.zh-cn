---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: "准备帐户合作伙伴在客户端计算机"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c5bdcb0a80b15a1905109229ddd20ee642a8dd7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-client-computers-in-the-account-partner"></a>准备帐户合作伙伴在客户端计算机

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

准备的访问权限的 Active Directory 联合身份验证服务联合 \(AD FS\) 应用程序的客户端计算机管理员帐户合作伙伴组织中的最简单方法是使用组策略。 组策略，可以方便地为你推送特定证书和所需的联盟可用于访问联盟应用程序中的所有客户端计算机的设置。  
  
以便客户端计算机可以无缝访问不证书提示或受信任的站点相关提示联盟应用程序，我们建议先准备每个客户端计算机部署之前广告 FS 广泛地在你的组织。 请考虑使用组策略为自动：  
  
-   配置每个要信任帐户联盟服务器的客户端计算机上的 Internet Explorer。  
  
    有关详细信息，请参阅[将配置客户端计算机信任帐户联盟服务器](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)。  
  
-   安装相应帐户联盟服务器、资源联合身份验证的服务器和 Web 服务器的安全套接字层 \(SSL\) 证书 \（或等效的证书链为受信任的 root\）客户端中的每台计算机上。  
  
    有关详细信息，请参阅[分发给客户端计算机使用组策略的证书](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)。  
  

## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
