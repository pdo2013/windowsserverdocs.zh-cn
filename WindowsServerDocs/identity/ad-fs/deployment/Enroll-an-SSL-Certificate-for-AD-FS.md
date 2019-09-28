---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: 为 AD FS 注册 SSL 证书
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: efa7c7aee848a5bbb68d3ce7140e135d37c2161d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408364"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>为 AD FS 注册 SSL 证书

Active Directory 联合身份验证服务 @no__t，在联合服务器场中的每台联合服务器上，需要安全套接字层 \(SSL @ no__t server authentication 的证书。 在场中的每台联合服务器上都可以使用相同的证书。 你必须获得该证书及其私钥。 例如，如果你将该证书及其私钥保存在一个 .pfx 文件中，则可以将该文件直接导入 Active Directory 联合身份验证服务配置向导。 此 SSL 证书必须包含以下内容：  
  
1.  "使用者名称" 和 "使用者备用名称" 必须包含联合身份验证服务名称，例如 fs.contoso.com。  
  
2.  使用者可选名称必须包含值**enterpriseregistration** ，后跟组织的用户主体名称 \(UPN @ no__t-2 后缀，例如**enterpriseregistration.corp.contoso.com**。  
  
    > [!WARNING]  
    > 如果计划为 Workplace Join 启用设备注册服务，请指定 "使用者备用名称" \(DRS @ no__t-1。  
  
> [!IMPORTANT]  
> 如果你的组织使用多个 UPN 后缀，并且你计划启用 DRS，则 SSL 证书必须包含每个后缀的 "使用者可选名称" 条目。  
  
## <a name="see-also"></a>请参阅
[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联合服务器场](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

