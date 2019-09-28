---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Web SSO 设计
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d7f52cd36588a1e5de4536a760c38c147dd1e003
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407833"
---
# <a name="web-sso-design"></a>Web SSO 设计

在 Web no__t-0Sign @ no__t-1On 中的 Web Single @-@-Active Directory 联合身份验证服务 @no__t \(SSO FS @ no__t-5 中，用户必须只进行一次身份验证，才能访问多个 AD FS @ 4AD-no__t 应用程序或服务。 在此设计中，所有用户都是外部用户，并且不存在联合信任，因为没有伙伴组织。 通常，当你想要通过 Internet 为单个使用者或客户提供对一个或多个 AD FS 保护的服务或应用程序的访问权限时，你可以部署此设计，如下图所示。  
  
![web sso 设计](media/adfs2_WebSSODesign.gif)  
  
利用 Web SSO 设计，通常在外围网络中托管 AD FS @ no__t-0secured 应用程序或服务的组织可以在外围网络中维护一个单独的客户帐户存储，这样可以更轻松地将客户帐户与员工帐户。  
  
你可以使用 Active Directory 域服务 \(AD DS @ no__t、SQL Server 或自定义属性存储来管理外围网络中的客户的本地帐户。  
  
这种设计与 [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)中的部署目标一致。  
  
有关可用于规划和部署 Web SSO 设计的详细任务的列表，请参阅 [Checklist：实现 Web SSO 设计 @ no__t-0。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
