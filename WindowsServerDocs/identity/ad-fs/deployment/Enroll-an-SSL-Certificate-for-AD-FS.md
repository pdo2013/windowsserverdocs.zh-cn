---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: 为 AD FS 注册 SSL 证书
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 12f544ad0d037c4ae7a9789238186b7ded311bdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825238"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>为 AD FS 注册 SSL 证书

>适用于：Windows Server 2016, Windows Server 2012 R2

Active Directory 联合身份验证服务\(AD FS\)需要安全套接字层证书\(SSL\)联合服务器场中每个联合身份验证服务器上的服务器身份验证。 场中每个联合身份验证服务器上，可以使用相同的证书。 你必须获得该证书及其私钥。 例如，如果你将该证书及其私钥保存在一个 .pfx 文件中，则可以将该文件直接导入 Active Directory 联合身份验证服务配置向导。 此 SSL 证书必须包含以下内容：  
  
1.  使用者名称和使用者可选名称必须包含联合身份验证的服务名称，例如 fs.contoso.com。  
  
2.  使用者可选名称必须包含值**enterpriseregistration**用户主体名称后跟\(UPN\)你的组织后缀，例如， **enterpriseregistration.corp.contoso.com**。  
  
    > [!WARNING]  
    > 如果打算启用设备注册服务指定使用者可选名称\(DRS\)工作区加入。  
  
> [!IMPORTANT]  
> 如果你的组织使用多个 UPN 后缀，并且你计划启用 DRS，SSL 证书必须包含一个使用者可选名称为每个后缀。  
  
## <a name="see-also"></a>请参阅
[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联合服务器场](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

