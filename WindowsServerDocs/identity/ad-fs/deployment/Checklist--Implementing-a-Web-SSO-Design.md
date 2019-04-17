---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: "清单-实现 Web SSO 设计"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 265daf3acb9632aa92f85962abc44a6a9ea8dfed
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-web-sso-design"></a>清单：实施 Web SSO 设计

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

此家长清单包括 cross\ 参考指向有关 Web Single\-上 Sign\ \(SSO\) 设计的 Active Directory 联合身份验证服务 \(AD FS\) 重要的概念。 它还包含链接附属将帮助你完成任务，实现这设计所需的清单。  
  
> [!NOTE]  
> 完成此清单订单中的任务。 当参考链接可将你带到概念主题或附属清单时，返回到本主题之后你查看概念主题或者完成的任务附属清单，以便你可以继续进行此清单中的其余任务。  
  
![Web sso](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**清单：实施 Web SSO 设计**  
  
||任务|参考|  
|-|--------|-------------|  
|![Web sso](media/icon_checkboxo.gif)|了解有关 Web SSO 设计重要的概念，并确定哪些广告 FS 部署目标，你可以使用自定义此设计以满足你的组织的需求。 **注意：**|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SSO 设计的 Web](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[识别您的广告 FS 部署目标](https://technet.microsoft.com/library/dd807053.aspx)|  
|![Web sso](media/icon_checkboxo.gif)|查看硬件、软件、证书，域名系统 \(DNS\)、特性官方商城和部署广告 FS 在你的组织的客户端要求。|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[附录 a：查看广告 FS 要求](https://technet.microsoft.com/library/ff678034.aspx)|  
|![Web sso](media/icon_checkboxo.gif)|根据你设计的套餐，安装一个或多个联合身份验证的服务器，在公司的网络或外围网络。 **注意：** Web SSO 设计需要只有一个联合身份验证的服务器正常工作。 一个联盟服务器以索赔提供商角色和信赖方角色进行操作。|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：设置联盟服务器](Checklist--Setting-Up-a-Federation-Server.md)|  
|![Web sso](media/icon_checkboxo.gif)|\(Optional\) 确定你的组织是否需要联合服务器代理外围网络中的。|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：向上联合服务器代理的设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![Web sso](media/icon_checkboxo.gif)|具体取决于你的 Web SSO 设计套餐，你要使用它的方式添加相应特性官方商城，依赖方信任，索赔，而规则联合身份验证服务。|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：配置的帐户的合作伙伴公司](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![Web sso](media/icon_checkboxo.gif)|如果你是管理员资源合作伙伴组织中, claims\ 启用你的 Web 浏览器应用程序、Web 服务的应用程序或 Microsoft® Office SharePoint® 服务器应用程序使用 WIF 和 WIF SDK。 **注意：**|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows 身份基础](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows 身份基础 SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
