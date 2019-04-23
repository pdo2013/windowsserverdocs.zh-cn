---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 清单-实现 Web SSO 设计
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 265daf3acb9632aa92f85962abc44a6a9ea8dfed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864038"
---
# <a name="checklist-implementing-a-web-sso-design"></a>清单：实现 Web SSO 设计

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

此父清单包含交叉\-引用链接到有关 Web 单一的重要概念\-符号\-上\(SSO\) Active Directory 联合身份验证服务的设计\(AD FS\). 它还包含指向从属清单的链接，该清单将帮助你完成实现此设计所需的任务。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当引用链接将你带到概念主题或从属清单时，在查看概念主题或完成从属清单中的任务后，请返回到本主题，这样你就可以继续完成此清单中的剩余任务。  
  
![web sso](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**核对清单：实现 Web SSO 设计**  
  
||任务|参考|  
|-|--------|-------------|  
|![web sso](media/icon_checkboxo.gif)|查看有关 Web SSO 设计的重要概念并确定哪些 AD FS 部署目标，可用于自定义此设计以满足组织的需求。 **注意：** |![web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Web SSO 设计](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[标识 AD FS 部署目标](https://technet.microsoft.com/library/dd807053.aspx)|  
|![web sso](media/icon_checkboxo.gif)|查看硬件、 软件、 证书、 域名系统 \(DNS\), ，属性存储和您的组织中部署 AD FS 的客户端要求。|![web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[附录 a:查看 AD FS 要求](https://technet.microsoft.com/library/ff678034.aspx)|  
|![web sso](media/icon_checkboxo.gif)|根据你的设计规划，企业网络中或外围网络中安装一个或多个联合服务器。 **注意：** Web SSO 设计要求只有一个联合身份验证服务器已成功运行。 单个联合身份验证服务器中的声明提供程序角色和信赖方角色的作用。|![web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：设置联合身份验证服务器](Checklist--Setting-Up-a-Federation-Server.md)|  
|![web sso](media/icon_checkboxo.gif)|\(可选\)确定你的组织是否需要在外围网络中的联合身份验证服务器代理。|![web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![web sso](media/icon_checkboxo.gif)|根据你的 Web SSO 设计规划和要使用它的方式，请将相应的属性存储、信赖方信任、声明和声明规则添加到联合身份验证服务。|![web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：配置帐户伙伴组织](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![web sso](media/icon_checkboxo.gif)|如果您是资源伙伴组织中的管理员，声明\-启用 Web 浏览器应用程序、 Web 服务应用程序或使用 WIF 和 WIF SDK 的 Microsoft® Office SharePoint® Server 应用程序。 **注意：** |![web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
