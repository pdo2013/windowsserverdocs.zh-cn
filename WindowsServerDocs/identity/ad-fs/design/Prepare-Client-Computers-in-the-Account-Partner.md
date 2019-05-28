---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: 在帐户伙伴中准备客户端计算机
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e3a746ec003cf312ffe0b9804f84a55c98aa8089
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190988"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>在帐户伙伴中准备客户端计算机

管理员帐户中的最简单方法伙伴组织准备客户端计算机到 Active Directory 联合身份验证服务的访问\(AD FS\)联合应用程序是使用组策略。 组策略为你提供一种便捷的方法，可让你将联合所需的特定证书和设置推送到将用于访问联合应用程序的所有客户端计算机。  
  
以便客户端计算机可无缝访问联合应用程序而无需证书提示或受信任的站点的相关提示，我们建议你首先准备每台客户端计算机，在部署 AD FS 之前广泛在组织中。 请考虑使用组策略来自动：  
  
-   每个客户端计算机信任的帐户联合身份验证服务器上配置 Internet Explorer。  
  
    有关详细信息，请参阅 [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)。  
  
-   安装相应的帐户联合身份验证服务器、 资源联合身份验证服务器和 Web 服务器安全套接字层\(SSL\)证书\(等效项链接到受信任的根证书或\)上每个客户端计算机。  
  
    有关详细信息，请参阅[将证书分发到客户端计算机使用组策略](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)。  
  

## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
