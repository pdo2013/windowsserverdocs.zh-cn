---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: "注册广告 FS SSL 证书"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 12f544ad0d037c4ae7a9789238186b7ded311bdf
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>注册广告 FS SSL 证书

>适用于：Windows Server 2016，Windows Server 2012 R2

Active Directory 联合身份验证服务 \(AD FS\) 需要联盟服务器电场的日落中的每个联盟服务器上的安全套接字层 \(SSL\) 服务器身份验证的证书。 在场中每个联合身份验证的服务器上，可以使用同一个证书。 你必须证书并提供其专用键。 例如，如果你拥有的证书和其专用键.pfx 文件中，你可以将文件直接导入 Active Directory 联合身份验证服务配置向导。 此 SSL 证书必须包含以下信息：  
  
1.  主题名称和者备用名称必须包含你联合身份验证服务名称，如 fs.contoso.com。  
  
2.  主题替代名称必须包含值**enterpriseregistration**后跟你的组织的用户主要名称 \(UPN\) 职务等**enterpriseregistration.corp.contoso.com**。  
  
    > [!WARNING]  
    > 如果你计划启用工作区加入为设备注册服务 \(DRS\)，指定者备用名称。  
  
> [!IMPORTANT]  
> 如果你的组织中使用多个 UPN 后缀，并且你打算启用 DRS，SSL 证书必须包含每个职务主题替代名称条目。  
  
## <a name="see-also"></a>请参阅
[广告 FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012R2 广告 FS 部署指导](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联盟服务器电场的日落](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

