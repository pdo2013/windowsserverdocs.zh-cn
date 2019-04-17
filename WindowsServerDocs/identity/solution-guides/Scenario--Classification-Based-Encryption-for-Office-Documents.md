---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: "Office 文档方案分类基于加密"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 38e058f36522ba6a2c81694cb883d0946b04adda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Office 文档的方案：基于分类的加密

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

保护敏感信息的主要是降低组织的风险。 各种法规，如医疗保险责任法案 (HIPAA) 和支付卡 Industry 数据安全标准 (PCI-DSS) 口述加密的信息，并原因有很多业务加密业务敏感信息。 但是，加密信息较高，并且它可能会影响业务工作效率。 因此，企业倾向于不同的方法和加密其信息的优先级。  
  
## <a name="BKMK_OVER"></a>方案说明  
 Windows Server 2012 提供自动加密敏感 Microsoft Office 文件，根据他们分类的能力。 这是通过调用 Active Directory Rights Management Services (广告 RMS) 保护的敏感文档几秒钟后，该文件被标识为敏感文件文件服务器上的文件管理任务。 这得益于连续文件文件服务器管理任务。  
  
广告 RMS 加密提供一层保护的文件。 即使无意中，有权访问敏感文件某人发送该文件通过电子邮件，该文件受广告 RMS 加密。 想要访问文件的用户首先验证到广告 rms 接收解密密钥自身。 下图显示了此过程。  
  
![解决方案指南](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**图 6**分类基于 RMS 保护  
  
非 Microsoft 文件格式的支持已非 Microsoft 供应商提供。 文件受广告 RMS 加密后，基于搜索或内容的分类如数据管理功能不再适用于该文件。  
  
## <a name="in-this-scenario"></a>在此情况下  
以下是适用于这种情况下的指南：  
  
-   [规划注意事项加密的 Office 文档](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [部署的 Office 文件 & #40 加密; 演示步骤 & #41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>负责并此方案中所含功能  
下表列出的角色和属于这种情况下的功能，并介绍了如何支持。  
  
|角色/功能|它如何支持此方案|  
|-----------------|---------------------------------|  
|Active Directory 域服务角色 (广告 DS)|广告 DS 提供存储，并管理目录启用应用程序从网络资源和应用程序相关数据信息的分布式的数据库。 在此情况下，在 Windows Server 2012 的广告 DS 引入了可以创建用户索赔和设备索赔复合身份（用户以及设备索赔）、新中心访问策略模型，，在授权决定文件分类信息的使用的索赔基于授权平台。|  
|文件和存储服务的作用<br /><br />文件服务器资源管理器|文件和存储服务提供的技术来帮助你设置并管理你的网络，你可以在此处存储文件并将它们与用户共享提供中心位置的一个或多个文件服务器。 如果您的网络的用户需要访问相同的文件和应用程序，或者备份和文件的集中的管理很重要，对你的组织，你应该设置一个或多个计算机文件服务器，在作为通过向计算机添加的文件和存储服务的角色和相应角色服务。 在此情况下，文件服务器管理员可以配置调用广告 RMS 保护敏感文档几秒钟后，该文件被标识为敏感文件文件服务器（连续文件上文件服务器管理任务）上的文件管理任务。|  
|Active Directory Rights Management Services(广告 RMS) 角色|广告 RMS 使个人和管理员（通过信息版权管理 (IRM) 策略）若要指定的文档、工作簿和演示文稿的访问权限。 打印、转发，或复制由未经授权的人员，这有助于防止敏感信息。 通过使用 IRM 限制了权限的文件后，因为权限的文件存储在该文档文件本身无论信息位于何处强制访问和使用情况的限制。 在此情况下，广告 RMS 加密提供一层保护的文件。 即使无意中，有权访问敏感文件某人发送该文件通过电子邮件，该文件受广告 RMS 加密。 想要访问文件的用户首先验证到广告 rms 接收解密密钥自身。|  
  


