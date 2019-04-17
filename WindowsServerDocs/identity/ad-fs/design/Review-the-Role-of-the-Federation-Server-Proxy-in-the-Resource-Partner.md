---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: "查看联合服务器代理资源伙伴中的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 31e285e863e4316a8e0a65f9b68c27442290927d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>查看联合服务器代理资源伙伴中的角色

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

联合服务器代理 Active Directory 联合身份验证服务 \(AD FS\) 中的可以在一个或多个以下角色，具体取决于你如何配置服务器以满足资源合作伙伴公司的功能：  
  
-   **帐户合作伙伴发现**: Internet 客户端计算机必须确定哪一个帐户合作伙伴将进行身份验证它。 客户端使用帐户合作伙伴发现 Web 表单 \(discoverclientrealm.aspx\)，它存储在资源合作伙伴联盟服务器代理服务器上查找帐户合作伙伴。 如果有多个帐户合作伙伴中 snap\，drop\ 减小菜单出现后便会向 Internet 访问帐户合作伙伴发现 Web 表单的客户端计算机的所有可用的帐户合作伙伴的客户端到广告 FS 管理配置。 你可以更改帐户合作伙伴发现 Web 表单通过自定义 discoverclientrealm.aspx 文件的提供给客户端计算机。  
  
-   **安全令牌重定向**：联合服务器代理帐户伙伴中的将安全令牌发送到资源的合作伙伴。 资源联合身份验证的服务器代理接受这些标记，并将其传输到联盟服务器资源伙伴中。 然后，资源联盟服务器问题送达特定资源的 Web 服务器的安全标记。 资源联合身份验证的服务器代理然后重定向到客户端的标记。  
  
总结，资源联盟服务器代理通过将客户端计算机重定向到可以进行身份验证的客户端联盟服务器便于联盟的登录过程。 资源联盟服务器代理还作为的代理客户端安全令牌资源联合身份验证服务。  
  
> [!NOTE]  
> 有必要，以帮助减少硬件和所需的证书大量时，可以位于联合身份验证的服务器代理 Web 服务器为同一台计算机上。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)

