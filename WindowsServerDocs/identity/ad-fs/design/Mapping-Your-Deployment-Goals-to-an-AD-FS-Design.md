---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: 将部署目标映射到 AD FS 设计
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866818"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>将部署目标映射到 AD FS 设计

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

查看现有的 Active Directory 联合身份验证服务完后\(AD FS\)部署目标并确定哪些目标相关的部署，可以将这些目标映射到特定的 AD FS 设计。 有关 AD FS 的详细信息预定义的部署目标，请参阅[标识 AD FS 部署目标](Identifying-Your-AD-FS-Deployment-Goals.md)。  
  
使用下表来确定哪些 AD FS 设计映射到适当的 AD FS 组合为你的组织的部署目标。 此表仅指两个主 AD FS 设计中，如本指南中所述。 但是，您可以通过使用 AD FS 部署目标的任意组合以满足你的组织的需求创建混合或自定义 AD FS 设计。  
  
|AD FS 部署目标|[Web SSO 设计](Web-SSO-Design.md)|[联合的 Web SSO 设计](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[对声明感知应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|否|是，在帐户伙伴中|  
|[向应用程序和服务的其他组织提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|否|是，在帐户伙伴中（可选）|  
|[另一个组织 Access 中的用户提供对声明感知应用程序和服务](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|是|是|  

## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

