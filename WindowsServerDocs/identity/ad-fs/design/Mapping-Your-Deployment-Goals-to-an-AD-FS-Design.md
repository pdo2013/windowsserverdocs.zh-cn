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
ms.openlocfilehash: 13d8ae8b8f3e4c8160f61284e5fb97e21b6a51b6
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191246"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>将部署目标映射到 AD FS 设计


查看现有的 Active Directory 联合身份验证服务完后\(AD FS\)部署目标并确定哪些目标相关的部署，可以将这些目标映射到特定的 AD FS 设计。 有关 AD FS 的详细信息预定义的部署目标，请参阅[标识 AD FS 部署目标](Identifying-Your-AD-FS-Deployment-Goals.md)。  
  
使用下表来确定哪些 AD FS 设计映射到适当的 AD FS 组合为你的组织的部署目标。 此表仅指两个主 AD FS 设计中，如本指南中所述。 但是，您可以通过使用 AD FS 部署目标的任意组合以满足你的组织的需求创建混合或自定义 AD FS 设计。  
  
|AD FS 部署目标|[Web SSO 设计](Web-SSO-Design.md)|[联合 Web SSO 设计](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[为声明感知应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|否|是，在帐户伙伴中|  
|[为其他组织的应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|否|是，在帐户伙伴中（可选）|  
|[为另一个组织中的用户提供对声明感知应用程序和服务的访问权限](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|是|是|  

## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

