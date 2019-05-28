---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Web SSO 设计
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 29d50a4d1855e609b6ac9ee627256201074a5033
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190721"
---
# <a name="web-sso-design"></a>Web SSO 设计

在 Web 单一\-符号\-上\(SSO\) Active Directory 联合身份验证服务中的设计\(AD FS\)，用户必须进行身份验证一次只能访问多个 AD FS\-受保护的应用程序或服务。 在此设计中，所有用户都是外部用户，并且不存在联合信任，因为没有伙伴组织。 通常情况下，你将部署此设计时您想要提供单个使用者或客户访问一个或多个 AD FS 保护的服务或应用程序通过 Internet，如下图中所示。  
  
![web sso 设计](media/adfs2_WebSSODesign.gif)  
  
使用 Web SSO 设计中，组织的通常托管受 AD FS\-受保护的应用程序或外围网络中的服务可以维护一个单独的外围网络，因此可以轻松地隔离客户中的客户帐户存储帐户与员工帐户。  
  
可以在外围网络中的客户管理的本地帐户，通过使用 Active Directory 域服务\(AD DS\)，SQL Server 或自定义属性存储。  
  
这种设计与 [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)中的部署目标一致。  
  
有关可用于计划和部署 Web SSO 设计的详细任务的列表，请参阅[核对清单：实现 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
