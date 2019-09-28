---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 清单-实现 Web SSO 设计
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8488e0c9195930374aacd959e72d0eff34142ca7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408466"
---
# <a name="checklist-implementing-a-web-sso-design"></a>清单：实现 Web SSO 设计

此父清单包含跨 @ no__t-0reference 链接，这些链接指向有关 Web Single @ no__t-1Sign @ no__t-2On \(SSO @ no__t-4 设计 for Active Directory 联合身份验证服务 \(AD FS @ no__t。 它还包含指向从属清单的链接，该清单将帮助你完成实现此设计所需的任务。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当引用链接将你带到概念主题或从属清单时，在查看概念主题或完成从属清单中的任务后，请返回到本主题，这样你就可以继续完成此清单中的剩余任务。  
  
@no__t 0web sso @ no__t-1Checklist：实现 Web SSO 设计**  
  
||任务|参考|  
|-|--------|-------------|  
|![web sso](media/icon_checkboxo.gif)|查看有关 Web SSO 设计的重要概念，并确定可以用于自定义此设计以满足组织需求的 AD FS 部署目标。 **注意：**|@no__t 0web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[WEB Sso 设计](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[标识 AD FS 部署目标](https://technet.microsoft.com/library/dd807053.aspx)的 @no__t 0web sso|  
|![web sso](media/icon_checkboxo.gif)|查看硬件、 软件、 证书、 域名系统 \(DNS\), ，属性存储和您的组织中部署 AD FS 的客户端要求。|@no__t 0web sso @ no__t-1Appendix A：查看 AD FS 要求](https://technet.microsoft.com/library/ff678034.aspx)|  
|![web sso](media/icon_checkboxo.gif)|根据你的设计规划，请在企业网络或外围网络中安装一个或多个联合服务器。 **注意：** Web SSO 设计只需一个联合服务器就能成功运行。 单个联合服务器在声明提供方角色和信赖方角色中起作用。|@no__t 0web sso @ no__t-1Checklist：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)|  
|![web sso](media/icon_checkboxo.gif)|\(Optional @ no__t-1 确定你的组织是否需要外围网络中的联合服务器代理。|@no__t 0web sso @ no__t-1Checklist：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![web sso](media/icon_checkboxo.gif)|根据你的 Web SSO 设计规划和要使用它的方式，请将相应的属性存储、信赖方信任、声明和声明规则添加到联合身份验证服务。|@no__t 0web sso @ no__t-1Checklist：配置帐户伙伴组织 @ no__t-0|  
|![web sso](media/icon_checkboxo.gif)|如果你是资源伙伴组织中的管理员，请使用 WIF 和 WIF SDK 声明 @ no__t-0enable 你的 Web 浏览器应用程序、Web 服务应用程序或 Microsoft® Office SharePoint® Server 应用程序。 **注意：**|@no__t 0web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />@no__t 0web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity FOUNDATION SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
