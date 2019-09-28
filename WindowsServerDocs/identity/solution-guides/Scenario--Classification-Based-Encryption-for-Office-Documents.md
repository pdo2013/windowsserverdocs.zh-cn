---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: 基于方案分类的 Office 文档加密
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ab1b277b83369cf2e4ef4be5aa467dea8b2d2f84
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357441"
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>场景：基于分类的 Office 文档加密

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

敏感信息的保护主要与为组织降低风险有关。 诸如 Health Insurance Portability and Accountability Act (HIPAA) 和支付卡行业数据安全标准 (PCI-DSS) 等各种合规性规章制度，都规定了信息加密，而且出于诸多商业考虑也需要加密敏感的业务信息。 但是，加密信息的成本比较高，这可能会损害企业的生产力。 因此，对于信息加密，组织往往采用不同的方法和优先级。  
  
## <a name="BKMK_OVER"></a>方案描述  
 Windows Server 2012 提供了根据用户的分类自动加密敏感 Microsoft Office 文件的功能。 这通过文件管理任务来完成，文件管理任务在文件被识别为文件服务器上的敏感文件后几秒钟，为敏感文档调用 Acitve Directory 权限管理服务 (AD RMS) 保护。 文件服务器上的连续文件管理任务可帮助实现该过程。  
  
AD RMS 加密为文件提供了另一层保护。 即使拥有某一敏感文件访问权限的人员在不经意间将该敏感文件通过电子邮件发送出去，该文件依然会受到 AD RMS 加密的保护。 要访问该文件的用户必须先通过 AD RMS 服务器的身份验证才能收到解密密钥。 下图显示了这个过程。  
  
![解决方案指南](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**图 6** 基于分类的 RMS 保护  
  
非 Microsoft 供应商可提供对非 Microsoft 文件格式的支持。 在文件受到 AD RMS 加密的保护之后，数据管理功能（如基于搜索或基于内容的分类）便不再适用于该文件。  
  
## <a name="in-this-scenario"></a>本方案内容  
以下是可用于此方案的指南：  
  
-   [Office 文档加密的规划注意事项](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [部署 Office 文件&#40;的加密演示步骤&#41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>此方案中包含的角色和功能  
下表列出了作为本方案组成部分的角色和功能，并描述了它们如何为本方案提供支持。  
  
|角色/功能|如何支持本方案|  
|-----------------|---------------------------------|  
|Active Directory 域服务角色 (AD DS)|AD DS 提供了一个分布式数据库，该数据库可以存储和管理有关网络资源的信息，以及启用了目录的应用程序中特定于应用程序的数据。 在此方案中，Windows Server 2012 中的 AD DS 引入了基于声明的授权平台，该平台允许创建用户声明和设备声明、复合标识（用户加设备的声明）、新的中心访问策略模型以及使用验证决策中的文件分类信息。|  
|“文件和存储服务”角色<br /><br />文件服务器资源管理器|文件和存储服务提供了可帮助设置和管理一台或多台文件服务器的技术，这些服务器提供了你可在网络上集中存储文件并与用户一起共享的位置。 如果你的网络用户需要访问相同的文件和应用程序，或如果集中备份和文件管理对于你的组织非常重要，则应该通过向计算机添加文件和存储服务角色和相应的角色服务的方式，将一个或多个计算机设置为文件服务器。 在本方案中，文件服务器管理员可以配置文件管理任务，该任务会在文件服务器上的某个文件被确定为敏感文件几秒钟之后，为敏感文档调用 AD RMS 保护（文件服务器上的连续文件管理任务）。|  
|Active Directory 权限管理服务 (AD RMS) 角色|AD RMS 允许个人和管理员（通过信息权限管理 (IRM) 策略）指定对文档、工作簿和演示的访问权限。 这就有助于防止敏感信息被未经授权的人打印、转发或复制。 在允许通过使用 IRM 文件限制文件后，无论信息在什么位置都会强制执行访问和使用限制，因为文件的权限存储在文档文件内。 在本方案中，AD RMS 加密为文件提供了另一层保护。 即使拥有某一敏感文件访问权限的人员在不经意间将该敏感文件通过电子邮件发送出去，该文件依然会受到 AD RMS 加密的保护。 要访问该文件的用户必须先通过 AD RMS 服务器的身份验证才能收到解密密钥。|  
  


