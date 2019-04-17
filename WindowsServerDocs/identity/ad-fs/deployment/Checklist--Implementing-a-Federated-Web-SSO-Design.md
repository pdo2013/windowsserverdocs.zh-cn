---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: "清单-实现联盟的 Web SSO 设计"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1122ee3814f656a7229dc2946d7441d525de5ae2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>清单：实施联盟的 Web SSO 设计

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

此家长清单包括 cross\ 参考指向有关 Active Directory 联合身份验证服务 \(AD FS\) 的联合 Web Single\-Sign\-On \(SSO\) 设计重要的概念。 它还包含链接附属将帮助你完成任务，实现这设计所需的清单。  
  
> [!NOTE]  
> 完成此清单订单中的任务。 当参考链接可将你带到概念主题或附属清单时，返回到此主题后查看概念主题，或你完成的任务附属清单，以便你可以继续进行此清单中的其余任务。  
  
![联合 web sso](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**清单：实施联盟 Web SSO 设计**  
  
||任务|参考|  
|-|--------|-------------|  
|![联盟的 web sso](media/icon_checkboxo.gif)|查看有关联合 Web SSO 设计的重要概念，并确定哪些广告 FS 部署目标，你可以使用自定义此设计以满足你的组织的需求。|![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联盟 Web SSO 设计](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[识别您的广告 FS 部署目标](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[部署规划](https://technet.microsoft.com/library/dd807083.aspx)|  
|![联盟的 web sso](media/icon_checkboxo.gif)|查看硬件、软件、证书，域名系统 \(DNS\)、特性官方商城和部署广告 FS 在你的组织的客户端要求。|![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[附录 a：查看广告 FS 要求](https://technet.microsoft.com/library/ff678034.aspx)|  
|![联盟的 web sso](media/icon_checkboxo.gif)|了解有关索赔重要的概念，部署在这两个合作伙伴组织广告 FS 之前声称规则、特性存储和广告 FS 配置数据库。|![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[了解关键广告 FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![联盟的 web sso](media/icon_checkboxo.gif)|根据你设计的套餐，安装每个合作伙伴组织中的一个或多个联合身份验证的服务器。 **注意：**联合 Web SSO 的设计，您需要帐户合作伙伴公司中的至少一个联合服务器和资源的合作伙伴公司中至少一个联合服务器。|![联合 web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：设置联盟服务器](Checklist--Setting-Up-a-Federation-Server.md)|  
|![联盟的 web sso](media/icon_checkboxo.gif)|\(Optional\) 确定你的组织是否需要联合身份验证的服务器代理。 如果你设计的套餐连接的代理服务器时，你可以在每个合作伙伴公司安装一个或多个联合身份验证的服务器代理。|![联合 web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：向上联合服务器代理的设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![联盟的 web sso](media/icon_checkboxo.gif)|根据你设计的套餐共享证书，将配置客户端，这两个合作伙伴组织中配置信任关系，以便他们可以通过联盟信任通信。|![联合 web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：配置的帐户的合作伙伴公司](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![联合 web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：配置资源合作伙伴公司](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![联盟的 web sso](media/icon_checkboxo.gif)|如果你是管理员资源合作伙伴组织中, claims\ 启用你的 Web 浏览器应用程序、Web 服务的应用程序或 Microsoft® Office SharePoint® 服务器应用程序使用 WIF 和 WIF SDK。|![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows 身份基础](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![联合 web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows 身份基础 SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
