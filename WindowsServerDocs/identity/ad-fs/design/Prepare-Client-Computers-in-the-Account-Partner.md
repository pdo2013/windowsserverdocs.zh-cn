---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: 在帐户伙伴中准备客户端计算机
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5725f4a7761d08a25ee8c67c0568977e3646397e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407940"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>在帐户伙伴中准备客户端计算机

在帐户伙伴组织中，管理员为使客户端计算机能够访问 Active Directory 联合身份验证服务 @no__t FS @ no__t 联合应用程序准备客户端的最简单方法是使用组策略。 组策略为你提供一种便捷的方法，可让你将联合所需的特定证书和设置推送到将用于访问联合应用程序的所有客户端计算机。  
  
为了使客户端计算机无需证书提示或受信任的站点相关提示即可无缝访问联合应用程序，我们建议你先准备好每台客户端计算机，然后在组织中广泛部署 AD FS。 请考虑使用组策略来自动：  
  
-   将每台客户端计算机上的 Internet Explorer 配置为信任帐户联合服务器。  
  
    有关详细信息，请参阅 [配置客户端计算机信任的帐户联合身份验证服务器](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)。  
  
-   在每台客户端计算机上安装相应的帐户联合服务器、资源联合服务器和 Web 服务器安全套接字层 @no__t \(or 等效的证书，这些证书将链接到受信任的根 @ no__t。  
  
    有关详细信息，请参阅[使用组策略将证书分发到客户端计算机](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)。  
  

## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
